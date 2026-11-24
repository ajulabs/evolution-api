# patches/

`patch-package` patches for third-party dependencies. They are applied by the
`postinstall` hook (`package.json`) and re-applied inside the Docker builder,
which copies this directory before `npm ci` (`COPY ./patches ./patches`).

Keep this directory tracked even when there is no patch: removing it breaks the
Docker build, since the `COPY` above cannot resolve a path that does not exist.

## Current patches

- `baileys+7.0.0-rc14.patch` (AjuLabs build): the pairing fix from
  evolution-foundation/evolution-api#2759 (handles `companion_reg_refresh` and
  guards `link_code_companion_reg`), plus the `<status>` ack handler that 2.3.8
  shipped in its rc.9 patch (commits 2f06ff24 and 821d511b), ported to rc14.
