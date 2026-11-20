# Browser build spike (work in progress)

Status: boots to the main menu and plays GDI mission 1 in Chrome with the player's own data files.

`vanilla-conquer-web.patch` applies on top of [Vanilla Conquer](https://github.com/TheAssemblyArmada/Vanilla-Conquer)
(commit `9103a8e`), the community SDL2 port of EA's GPL v3 Tiberian Dawn source. It adds an Emscripten
(WebAssembly) build of Tiberian Dawn that runs in Chrome.

No game data is included. The page asks the player for their own original data files and keeps them in
the browser's IndexedDB.

## Build

    git clone https://github.com/TheAssemblyArmada/Vanilla-Conquer vc && cd vc
    git checkout 9103a8e && git apply ../vanilla-conquer-web.patch
    source /path/to/emsdk/emsdk_env.sh
    web/build.sh
    cd build-web && python3 -m http.server 8000   # open http://localhost:8000/vanillatd.html

## What the patch changes
- CMake: Emscripten SDL2 + OpenAL ports, ASYNCIFY link flags, custom HTML shell.
- `common/webyield.h`: periodically yields the game's blocking loops back to the browser.
- `ms_sleep`/`us_sleep` use `emscripten_sleep`.
- Browser defaults: windowed canvas, absolute mouse (no pointer lock), 1280x800.
- `Call_Back()` and timer reads yield so audio and input keep flowing.
- Data path is `/data` in the virtual file system (filled by the page from the player's files).
- `web/shell.html`: file picker, IndexedDB persistence, Play button (audio needs a click).

License: GPL v3, same as the source it modifies.
