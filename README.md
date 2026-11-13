# camo-gitian.sigs

Signatures for CAMO release builds. CAMO's binaries are built with [gitian](https://github.com/thedifference7717-sys/camo/tree/main/contrib/gitian):
anyone can rebuild them from the source code and get **byte-for-byte identical** files. Each builder publishes the
hashes they got, signed with their own key, in this repository. When independent builders agree, you know the
downloads on [camocoin.org](https://camocoin.org) were built from the public code and nothing else.

## Layout

```
gitian-pubkeys/<builder>.asc                     builders' public signing keys
v1.0.0-linux/<builder>/camo-linux-0.18-build.assert       hashes this builder got
v1.0.0-linux/<builder>/camo-linux-0.18-build.assert.sig   their signature over it
v1.0.0-win/...   v1.0.0-osx/...                   same for Windows and macOS
```

## CAMO's release key

`thedifference7717-sys` signs with the CAMO release key:

```
CAMO release signing <camocoin@proton.me>
5F1A 8350 C748 BC43 AD19  2472 AF13 7B28 E11A 0857
```

The same key is published on [keys.openpgp.org](https://keys.openpgp.org/search?q=5F1A8350C748BC43AD192472AF137B28E11A0857),
on camocoin.org and in the camo repository (`contrib/gitian/gitian-pubkeys/camo-release.asc`). Check that all three match.

## Verify a release

```bash
gpg --import gitian-pubkeys/*.asc
gpg --verify v1.0.0-linux/thedifference7717-sys/camo-linux-0.18-build.assert.sig
```

Then compare the SHA-256 lines in the `.assert` files of different builders: they must be identical, and must
match the `SHA256SUMS.asc` published with the release.

## Become a builder

Follow `contrib/gitian/CAMO-RELEASE.md` in the camo repository, build the same tag, and open a pull request here
adding `gitian-pubkeys/<you>.asc` and your signed `.assert` files. More independent builders make every release
more trustworthy.
