# Torto reader font resources

GitHub release: <https://github.com/TortoTech/lab/releases/tag/reader-fonts-v1>

Gitee mirror repository: <https://gitee.com/TortoTech/lab>

Reading fonts are release attachments, not website assets or application bundles.
Resource versions are independent of application versions. Version `1` contains
the unchanged LXGW WenKai GB Screen and Literata Roman and Italic used by Torto.

## Release contents

- `LXGWWenKaiGBScreen.ttf`
- `Literata-opsz-wght.ttf`
- `Literata-Italic-opsz-wght.ttf`
- `OFL-1.1.txt`
- `README.md` with source and copyright notices
- `manifest.json` with resource version, family/style, file size, SHA-256, GitHub
  asset IDs, and any verified Gitee mirror URLs

The font binaries remain in the Torto repository for offline regression tests
and for older clients that still download its pinned raw files. Do not remove
those historical resources.

## Manual publication

1. Stage the exact font files and license records. Verify each font against
   Torto's `assets/fonts/manifest.json` before uploading.
2. Create a draft GitHub release with a new resource tag. Upload the fonts and
   license records. Retrieve the numeric asset IDs and compare the returned
   sizes and SHA-256 digests with the local files.
3. Update the font manifest with those asset IDs and upload it. Publish the
   resource release, then anonymously download each font through
   `https://api.github.com/repos/TortoTech/lab/releases/assets/{asset_id}` with
   `Accept: application/octet-stream`. Follow redirects and check both size and
   SHA-256. Do not save temporary CDN redirect URLs in the manifest.
4. In the Gitee `TortoTech/lab` repository, manually create the same release tag,
   copy its release notes, and upload the exact same six files. This is a manual
   operation; do not add automatic Gitee synchronization Actions.
5. Copy the actual attachment download URLs from the Gitee release. Download
   each font without a login or token and compare it with the GitHub copy.
   Large font files must also pass this check; a successful logged-in browser
   download alone is insufficient.
6. Record the verified URLs in the client's per-font `mirror_url` fields.
   Until verification succeeds, omit a mirror rather than adding a broken or
   login-gated address. The client tries GitHub first, then the mirror on
   network, HTTP, size, checksum, or font validation failures.
7. Before shipping a client, keep its font manifest and the release manifest
   identical. Finish mirror configuration while preparing the release; never
   replace published font bytes or their asset IDs. For changed font bytes,
   create a new resource release and update the content version and hashes.

Moving an unchanged font to this repository must preserve its resource version,
ID, filename, and hash. This keeps existing application caches usable across
upgrades. Cache hits make no GitHub API requests; public font downloads need no
GitHub token.

## Verification

From a Torto checkout, run:

```sh
cargo build --locked -p rebook-desktop --bin torto
cargo test --locked -p rebook-desktop fonts::tests
cargo test --locked -p rebook-desktop fonts::tests::public_font_sources_match_pinned_content -- --ignored --exact
```

The last check performs real anonymous downloads from every configured source.
The regular tests cover asset request headers, redirects, fallback, corrupt or
partial responses, and persistent cache reuse without public network access.
