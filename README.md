# ExoPlayer [![](https://img.shields.io/github/v/release/hehang0/exoplayer-extension-flac.svg?label=latest)](https://github.com/HeHang0/exoplayer-extension-flac/releases) [![](https://jitpack.io/v/HeHang0/exoplayer-extension-flac.svg)](https://jitpack.io/#HeHang0/exoplayer-extension-flac)


A ExoPlayer extension for support flac.

## Using

Please refer to
[AndroidX Media](https://github.com/androidx/media/blob/release/README.md) for
the usage instructions of the latest release.

ExoPlayer modules can be obtained from [the Google Maven repository][]. It's
also possible to clone the repository and depend on the modules locally.

[the Google Maven repository]: https://developer.android.com/studio/build/dependencies#google-maven

### From the Google Maven repository

#### 1. Add the JitPack repository to your build file

``` gradle
repositories {
    mavenCentral()
    maven { url 'https://www.jitpack.io' }
}
```

#### 2. Add module dependencies

The easiest way to get started using ExoPlayer is to add it as a gradle
dependency in the `build.gradle` file of your app module. The following will add
a dependency to the full library:

```gradle
implementation 'com.github.HeHang0:exoplayer-extension-flac:1.3.0'
```

where `1.3.0` is your preferred version.

#### 3. Turn on Java 8 support

If not enabled already, you also need to turn on Java 8 support in all
`build.gradle` files depending on ExoPlayer, by adding the following to the
`android` section:

```gradle
compileOptions {
  targetCompatibility JavaVersion.VERSION_1_8
}
```

#### 4. Enable multidex

If your Gradle `minSdkVersion` is 20 or lower, you should
[enable multidex](https://developer.android.com/studio/build/multidex) in order
to prevent build errors.

### Using `LibflacAudioRenderer` with ExoPlayer

```java
ExoPlayer.Builder builder = ExoPlayer.Builder(context);
DefaultRenderersFactory factory = new DefaultRenderersFactory(context.getApplicationContext())
    .setExtensionRendererMode(DefaultRenderersFactory.EXTENSION_RENDERER_MODE_ON);
builder.setRenderersFactory(factory);
ExoPlayer player = builder.build();
```

* If you're passing a `DefaultRenderersFactory` to `ExoPlayer.Builder`, you
  can enable using the module by setting the `extensionRendererMode` parameter
  of the `DefaultRenderersFactory` constructor to
  `EXTENSION_RENDERER_MODE_ON`. This will use `LibflacAudioRenderer` for
  playback if `MediaCodecAudioRenderer` doesn't support the input format. Pass
  `EXTENSION_RENDERER_MODE_PREFER` to give `LibflacAudioRenderer` priority
  over `MediaCodecAudioRenderer`.
* If you've subclassed `DefaultRenderersFactory`, add a `LibflacAudioRenderer`
  to the output list in `buildAudioRenderers`. ExoPlayer will use the first
  `Renderer` in the list that supports the input media format.
* If you've implemented your own `RenderersFactory`, return a
  `LibflacAudioRenderer` instance from `createRenderers`. ExoPlayer will use
  the first `Renderer` in the returned array that supports the input media
  format.
* If you're using `ExoPlayer.Builder`, pass a `LibflacAudioRenderer` in the
  array of `Renderer`s. ExoPlayer will use the first `Renderer` in the list
  that supports the input media format.

## Links

* [Flac decoder module][]

[Flac decoder module]: https://github.com/androidx/media/tree/release/libraries/decoder_flac
