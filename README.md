# mastodon-comments-autopost
A workflow to automatically toot to mastodon upon a new (jekyll) post. Includes explanation on how to enable a integation of mastodon comments into the static blog

# Setup
> [!Warning]
> This was tested in combination with the minma theme, jekyll and github pages for anything else, things might break

1) Copy into the base of your jekyll folder:
 - .github
 - _layouts
 - into scripts/ copy https://github.com/dpecos/mastodon-comments/blob/master/mastodon-comments.js

> [!TIP]
> The last two steps are only needed if you want to enable the "comment system", if not just skip it and simply do not use the `post_comment` layout.

2) Generate the mastodon API token
in the mastodon account that should post generate a API token:
Under Settings -> Development click "New Application"
 - Give it a new name
 - you do not need to change the forwarding URI (TODO I do not know what this does, please write me, if you know)
 - give it the `write:statuses` permission
 - save the application
 - copy the access token (the other values are not needed)

3) Github actions environment
 - Under Settings -> `Secrets and Variables` -> Actions Create a new environment called `toot`
 - In this environment add a secret called `MASTODON_TOKEN` and paste the mastodon access token from above
 - enable github pages under Settings -> Pages and set the source to `Github actions`
 - Allow the actions write access to the repository, so it can write back the comment ids: Settings -> Actions under Workflow permissions enable `Read and write permission` (NOTE: If you know how to do this more fine grained, also please write me)

 4) change your `_config.yml` to include
```yaml
mastodon:
- username: overwhelming_complexity
  instance: troet.cafe
```
(Change the username and instance accordingly)

> [!NOTE]
> If you use the minima theme, this will also add a link to this account in the footer of your blog

That should be it, you should be ready to go! Feel free to open an issue in case something does not work. And here of course to blog post, about this project (TODO) because where would be the fun otherwise, right?


# Usage
After setup, just use the layout `post_comment` to enable comments under the post, it should not interfere with your existing layout, but I only tested this with the post layout of minima.

You will also likely want to change the message that is being posted, together with the link and title in l.164 to l.153 in `.github/scripts/toot_and_commit.py`

# NOTE: 
> [!WARNING]
> The generation of links only works with explicitely provided links (using the permalink frontmatter) or the default link schema (it will also not work if you explicitely set the default schema in the permalink frontmatter)

# TODO
- Make this easier to use, using git subsystems
- the python script may fail silently, because of weird github action python error code handling