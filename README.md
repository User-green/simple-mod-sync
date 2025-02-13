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

### Notes
- **IMPORTANT:** Entries in `sync` will be replaced on user PC if `version` changes IN ANY WAY.

- - Aditionaly, if `name` changes, than NEW FILE will be downloaded, WIHOUT removeing old one.

- - If you have to change mod `name` make shore that you add its old `name` to the `modify` is `remove` entry. **HOWEVER**, beware that *Simple Mod Sync* modifies filename as described [here](https://github.com/oxydien/simple-mod-sync?tab=readme-ov-file#tecnical-deatails). Take this into a count when creating `remove` entries.


- Add more enteries as needed to the `sync` and `modify` arrays as needed.

- If you want, you can remove `modify` array entierly. Do not forget to remove comma.

- If you want, you can remove/add any data in `sync` array, as long as section contains at least one entry. 
- - Note that data will not be removed on user PC as long as you dont explicely state removal in `modify`.

- Note, that `.` (current directory) is `.minecraft` (or whatever your directory with `server.jar` is called).

- Note that `remove` uses ReGeX for `pattern`.

- Beware of files been both in `sync` and `modify` at the same time, especialy with `modyfy/remove` action. For best stability remove files from `sync` before `modify`-ing them. 

- Config files must be `zip`s, containing whatever config files you want to push. 
-- Sometimes you may want to create subdirectory inside `./config/` (f.e. `config/jei/{FILES}`). HOWEVER. If you whant to sync files inside the `./config` (`./config/{FILES}`) directory, you should set `directory` to just `config`.
-- Note: The whole pathing system works like in linux, and so `./config` = `config`.

## Simple Setup Guide

1. **Create the JSON File**: Use the [example schema](#example-schema-file) to create your mod list file.
    NOTE: You can automate creation process useing scripts in the `translators` directory of this repo.
   
2. **Host the File**: Upload the JSON file to an HTTP server. You can use services like [Pastebin](https://pastebin.com) or spin up your own server for this.

3. **Install the Mod**: Install the _Simple Mod Sync_ mod as usual. When the game starts for the first time, it will prompt you to enter the URL of your JSON file.

4. **Monitor Synchronization**: Once the URL is set, the mod synchronization status will be visible in the top-left corner of the title screen.

- To change the URL later, simply update the `download_url` setting in the config file or in the synced mods menu.

## Tecnical deatails

SimpleModSync works without creating any aditional files (metadata), and so all data is stored in the mod filename.
This means that any `mods\resourcepack\datapack\shader`'s name will be built with following structure: `[contentName]-[contentVersion].[extension]`.
In said structure `[contentName]` equils `name` used in JSON, `[contentVersion]` - to the `version` used in JSON, and `[extention]` selected automaticly, based on `type` in JSON (`jar` for `mod`, `zip` for everythig else).

The only exeptin to this rule are `config`s, where `.zip` file is unziped into selected directory. There, metadata is created. Said metadata is located at `./[path]/sms_[contentName]-[contentVersion].json`, and contains anmes of all files extracted from arcive.
(`[path]` = JSON's `directory`, `[contentName]` = JSON's `name`, and `[contentVersion]` = JSON's `version`)

For any type of entry in `sync` ANY difference detected between `[contentVersion]` and JSON's `version` for a file wich `[contentName]` maches `name`, will cause new be, and old file - removed. 
For `config` arcives -- all contents of the old `config.zip` will be removed before new `config.zip` will be unziped. This will not effect files with different `path`es.

## For Developers

Want to contribute? Whether it's reporting an issue or submitting a pull request, your help is highly appreciated!

- [Open an Issue](https://github.com/oxydien/simple-mod-sync/issues/new)
- [Create a Pull Request](https://github.com/oxydien/simple-mod-sync/pulls)

## License

This mod is licensed under the MIT License. See the [LICENSE file](./LICENSE) for more details.
