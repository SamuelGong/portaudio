# Zhifeng's Guidance

FYI: [This](./README-orig.md) is the original README for the original portaudio project

## 1. Comment Steps

```bash
git clone git@github.com:SamuelGong/portaudio.git
cd portaudio
```

Also, [download](https://developer.android.com/codelabs/basic-android-kotlin-compose-install-android-studio#0) and install Android Studio (with NDK installed at SDK Manager).
Make the path to your installed NDK, e.g., `~/Library/Android/sdk/ndk/29.0.14206865` an environment variable, i.e.,,

```bash
export ANDROID_NDK=/Users/samuel/Library/Android/sdk/ndk/29.0.14206865  # Use your own path
```

Moreover, you need to confirm the ABI version of the target platform for cross-compilation.

```bash
adb shell getprop ro.product.cpu.abilist

# Example output: arm64-v8a
```

## 2. Compiling for Oneplus Phones (Android @ ARM) on a Macbook (macOS @ Apple Silicon)

Prerequisites:

```bash
brew install cmake ninja
```

Main steps (with option `DANDROID_ABI` properly set):

```bash
export API=26
rm -rf build-arm64-v8a

cmake -S . -B build-arm64-v8a -G Ninja \          
 -DCMAKE_TOOLCHAIN_FILE="$ANDROID_NDK/build/cmake/android.toolchain.cmake" \
 -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-$API \
 -DPA_BUILD_SHARED=ON -DPA_BUILD_STATIC=OFF -DBUILD_SHARED_LIBS=ON
cmake --build build-arm64-v8a --config Release

mkdir -p out/include out/lib
rsync -a include/ out/include/
cp build-arm64-v8a/libportaudio.so out/lib/
```

<details> <summary><b>Example file output (Tab here to expand)</b></summary>

```
out
├── include
│   ├── pa_asio.h
│   ├── pa_jack.h
│   ├── pa_linux_alsa.h
│   ├── pa_linux_pulseaudio.h
│   ├── pa_mac_core.h
│   ├── pa_win_ds.h
│   ├── pa_win_wasapi.h
│   ├── pa_win_waveformat.h
│   ├── pa_win_wdmks.h
│   ├── pa_win_wmme.h
│   └── portaudio.h
└── lib
    └── libportaudio.so
```

</details>