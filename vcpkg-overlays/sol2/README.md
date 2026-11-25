# sol2 3.5.0 overlay port

This is a minimal vcpkg overlay port for **sol2 3.5.0**, used by `macos-server/build.sh` through:

```text
-DVCPKG_OVERLAY_PORTS=<toolkit>/macos-server/vcpkg-overlays
```

## Why this exists

- BeamMP-Server v3.9.4 pins sol2 3.3.1, which does not compile with current Apple Clang (`optional<T&>::construct` is missing).
- sol2 3.5.0 compiles, but its `usertype_container.hpp` refers to `iter::end()`, while the type exposes `sen()`. Without a fix, `src/TLuaEngine.cpp` fails to build.
- `fix-iter.patch` changes `i.end()` to `i.sen()` during the vcpkg build, so installed headers are never edited by hand.

`portfile.cmake`, `vcpkg.json` and `header-only.patch` are based on the upstream vcpkg `sol2` port. `fix-iter.patch` is local to this toolkit.

Do not patch `vcpkg_installed/` manually; keep dependency fixes in this overlay.
