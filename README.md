# MOS extra apps

A personal, **unofficial** MOS app catalog: small self-hosted services that are useful to have on a server but will never be worth a place in the official [My Own Suite](https://github.com/rpuls/my-own-suite) catalog, because almost nobody needs them.

It is public so anyone can use it. That is not the same as it being reviewed. Nothing here has been through MOS's privacy assessment or any other check, the packages are maintained when their author happens to need them, and MOS will label every app installed from this repository **External · Unverified** for as long as it is installed. Read the package before you install it — that is the whole basis on which you would be trusting it.

## Add it to MOS

Paste this into the search box on the **Apps** screen in Suite Manager:

```
https://github.com/rpuls/MOS-extra-apps
```

MOS resolves the default branch to a commit, downloads that commit, and shows you every app this repository publishes along with what each one is asking for. Install one from there, or choose **Add this source** to keep the repository: its apps then sit on the Apps screen under their own heading, and the source appears in **Settings → App sources you added**, where it can be refreshed or removed.

Removing the source stops offering these apps and stops their updates. Anything already installed keeps running, with its data and settings untouched.

## What is in it

| App | What it does |
| --- | --- |
| [`epson2paperless`](.mos/epson2paperless/) | Asks an Epson scanner on your network for a scan and files it in Paperless-ngx. A plugin for the official `paperless-ngx` package: install that first and connect the two, and the Paperless address comes from the connection. Packages [`mtheuma/epson2paperless`](https://github.com/mtheuma/epson2paperless). Triggered by an HTTP request rather than by the scanner's own Scan button — [why](.mos/epson2paperless/README.md#what-this-package-cannot-do). |

## Layout

Everything MOS reads lives in `.mos/`, one folder per app, each folder named for the id its own `manifest.json` declares:

```
.mos/
└── epson2paperless/
    ├── manifest.json   # the package definition
    ├── Dockerfile      # pins the upstream image by digest
    ├── icon.svg        # catalog icon
    └── README.md       # technical reference
```

That shape is what makes this a catalog rather than a single app. A repository with `manifest.json` at the root of `.mos/` publishes exactly one app, and MOS never looks for package folders beside it; [MOS-external-app-example](https://github.com/rpuls/MOS-external-app-example) is that shape, and is the one to copy when packaging a project's own app.

Each app is validated on its own. One broken package is listed with its reasons rather than hidden, and does not cost the others their place.

## Adding an app here

1. Make `.mos/<id>/` and put a `manifest.json` in it whose `id` is that folder name.
2. Pin every base image by immutable digest. A floating tag would let the image change under a package version claiming to be unchanged.
3. Keep inside the constrained profile MOS applies to packages it has not reviewed: no `privileged`, `ports`, `network`/`networkMode`, `devices`, `capAdd`, `securityOpt`, no Docker socket, no host paths. Volumes are named.
4. Declare any file beyond `manifest.json`, `Dockerfile`, `Dockerfile.<service>`, `README.md`, `entrypoint.sh`, `icon.*`, and `privacy-review.json` in `manifest.packageFiles`, or the package is refused.
5. Set `minimumMosVersion` to the oldest MOS release the package's manifest fields actually work on — `0.20.0` for anything declaring `appVersion`.

The rules and the reasoning behind them are written up in [MOS-external-app-example](https://github.com/rpuls/MOS-external-app-example#the-rules-that-are-easy-to-get-wrong).

Note that MOS only learned to read a repository publishing more than one app in the release after 0.20.0. An older server will not find these packages at all: it looks for `.mos/manifest.json`, which is deliberately not here.

## License

MIT for the packaging in this repository. Each packaged application keeps its own license, which is the upstream project's to state.
