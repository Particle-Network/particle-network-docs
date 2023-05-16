# ❔ FAQ

## What networks do you support?

[👉 Check out our supported networks here](../overview/available-networks/)

## How do I integrate my own user ID system?

After the user successfully logs in, **Particle Auth** SDK will return the user **uuid** and **token**. You can then retrieve and verify the user info by calling [a server API](auth-service/sdks/server-api.md).

## What types of NFTs are supported?

**Particle Wallet** supports images and 3D models. For media types not supported, the default cover image will be displayed. Audio files and video files will be supported soon.

## Web SDK integration problems

### webpack 5 polyfills error

If your project use webpack 5, you need include polyfills for node.js core modules, add config to `webpack.config.js`

```typescript
resolve: {
    fallback: {
        crypto: require.resolve("crypto-browserify"),
        stream: require.resolve("stream-browserify"),
        buffer: require.resolve("buffer"),
        string_decoder: require.resolve("string_decoder/"),
    },
    plugins:[
        new webpack.ProvidePlugin({
            Buffer: ["buffer", "Buffer"],
        })
    ]
}
```

Then add packages to your project.

```
yarn add crypto-browserify stream-browserify buffer string_decoder
//or
npm install crypto-browserify stream-browserify buffer string_decoder
```



## Unity SDK integration problems

if your project use firebase tools,Refer to the following process

{% file src="../.gitbook/assets/Partcile Unity sdk with firebase tools .pdf" %}

## iOS SDK integration problems

## Android SDK integration problems



## Flutter SDK integration problems

## ReactNative SDK integration problems











