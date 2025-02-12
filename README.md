# Simple Mod Sync

A lightweight mod for synchronizing game mods via a URL-based schema.

## Features

- **Automated Mod Synchronization**: Synchronize mods from a specified URL on every game start.
- **Customizable Destination**: Choose where mods are downloaded on your system.
- **User-Friendly Setup**: Simple configuration with minimal setup required.

## Example schema file

The file retrieved from the URL must follow this structure:

```json
{
  "sync_version": 3,
  "sync": [
    {
      "url": "https://example.com/url/to/mod.jar",
      "name": "some-mod-name",
      "version": "anything",
      "type": "mod"
    },
    {
      "url": "https://example.com/url/to/resourcepack.zip",
      "name": "some-resourcepack-name",
      "version": "anything",
      "type": "resourcepack"
    },
    {
      "url": "https://example.com/url/to/datapack.zip",
      "name": "some-resourcepack-name",
      "version": "anything",
      "type": "datapack"
    },
    {
      "url": "https://example.com/url/to/shader.zip",
      "name": "some-shader-name",
      "version": "anything",
      "type": "shader"
    },
    {
      "url": "https://example.com/url/to/config.zip",
      "name": "some-config-name",
      "version": "anything",
      "type": "config",
      "directory": "config/SUBDIR"
    }
  ],
  "modify": [
    {
      "type": "remove",
      "pattern": "^usercache\\.json$",
      "path": "."
    }
  ]
}
```
> Add more enteries as needed to the `sync` and `modify` arrays as needed.
> If you want, you can remove `modify` array entierly. Do not forget to remove comma.
> If you want, you can remove/add any data in `sync` array, as long as section contains at least one entry. Note that data will not be removed on user PC as long as you dont explicely state removal in `modyfy`.
> Note, that `.` directory is `.minecraft` (or whatever your directory with `server.jar` is called).
> Note that `remove` uses ReGeX for `pattern`.
> Beware of files been bouth in `sync` and `modify` at the same time, as there might be dragons. For best stability remove files from `sync` before `modify`-ing them. 


## Simple Setup Guide

1. **Create the JSON File**: Use the [example schema](#example-schema-file) to create your mod list file.
    NOTE: You can automate creation process useing scripts in the `translators` directory of this repo.
   
2. **Host the File**: Upload the JSON file to an HTTP server. You can use services like [Pastebin](https://pastebin.com) or spin up your own server for this.

3. **Install the Mod**: Install the _Simple Mod Sync_ mod as usual. When the game starts for the first time, it will prompt you to enter the URL of your JSON file.

4. **Monitor Synchronization**: Once the URL is set, the mod synchronization status will be visible in the top-left corner of the title screen.

- To change the URL later, simply update the `download_url` setting in the config file or in the synced mods menu.

## For Developers

Want to contribute? Whether it's reporting an issue or submitting a pull request, your help is highly appreciated!

- [Open an Issue](https://github.com/oxydien/simple-mod-sync/issues/new)
- [Create a Pull Request](https://github.com/oxydien/simple-mod-sync/pulls)

## License

This mod is licensed under the MIT License. See the [LICENSE file](./LICENSE) for more details.
