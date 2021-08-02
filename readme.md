# rsync-my-site

This script helps you to syncronize your local website files to a remote host

## Usage
rsync-my-site is designed to be used in an npm project.

## Note
DELETION IS OPT-IN

In order to delete on remote, use the `-d` switch with the `up` command; deletion is a an "opt-in" feature.
```bash
rsync-my-site up -d
```

```json
npx rsync-my-site
```


Add to your package.json.scripts a script entry pointing to the script 
Add a config with the necessary keys

## Notes about package.json.config
### required properties:
* config.ssh_user AND config.ssh_domain, OR just config.ssh_alias
* config.ssh_remote_root
* config.ssh_local_root
* rsync_exclude_list[]
* rsync_protect_and_perish_list[].{protect[],.perish[]}
* rsync_include_list[]

### other
* config.ssh_local_root is ALWAYS relative to cwd. Trailing slash means to exclude the source directory, no trailing slash means to include the source directory

### How do the include and exclude lists affect what is uploaded from config.ssh_local_root?
If rsync_exclude_list[] is empty, rsync_include_list[] has no effect. The reason is that the latter specifies exceptions to the former.


For example, say source (path/to/upload/) contains the following:
```bash
upload/
├── a
├── b
├── c
├── d
├── e
├── f
├── g
└── something
    └── b
```

If config.rsync_exclude_list[] contains 'b', what's uploaded is the following:
```bash
./
a
c
d
e
f
g
something/
```

Notice that both /b and /something/b were excluded. If you prefer to exclude only the 'b' file at the root of the source, then add '/b' to config.rsync_exclude_list[].

Again, if config.rsync_exclude_list[] contains 'b', and config.rsync_include_list[] contains '/something/b', what's uploaded is the following:
```bash
./
a
c
d
e
f
g
something/
something/b
```
Notice that the include entry overrules the exclude entry.


Config properties in package.json
```javascript
{
    "config": {
        "ssh_alias": string,
        "ssh_user": string,
        "ssh_domain": string,
        "ssh_local_root": string,
        "ssh_remote_root": string,
        "rsync_exclude_list": [
            // files to exclude from ssh_local_root
        ],
        "rsync_protect_and_perish_list": {
            "protect": [
                // remote files that absolutely should not be deleted
            ],
            "perish": [
                // empty folders that contain only these can be deleted
            ]
        },
        "rsync_include_list": [
            // files to include from rsync_exclude_list
        ]
    }
}
```


For far greater detail than you'll likely ever need, please consult the man page!
- https://linux.die.net/man/1/rsync



# tasks
- [ ] make sure to stop if json is malformed!
- [ ] warn about empty include list too
- [ ] command to put website in an 'under construction' mode
- [ ] do i need to CD into directory before starting rsync?
- [ ] test running from project root
- [ ] test running from install 
- [ ] need install command
- [ ] script exit error messages should always be printed
- [ ] make sure warn and info commands print correctly
- [x] show details and prompt to continue
- [x] work with config.ssh_alias
- [x] handle script args better
- [x] deleting from remote should be opt-in
- [x] probably shouldnt exit for jq error(1)

## references
* https://clig.dev
* liberachat
