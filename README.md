<h1 align="center">
  WebKitGTK SDK
</h1>

<p align="center"><b>This is the repository for the WebkitGTK SDK and Content Snap.</b>It is a community-maintained package to easily allow other snaps use the latest webkitgtk-6+ related libraries without any hassle via snapcraft's <a href="https://snapcraft.io/docs/content-interface">content interface</a>.</p>

<p align="center"><i>"WebKitGTK is a full-featured port of the WebKit rendering engine, suitable for projects requiring any kind of web integration, from hybrid HTML/CSS applications to full-fledged web browsers."</i></p>

<b>WebKitGTK SDK Status</b>

<ul>
<a href="https://snapcraft.io/webkitgtk-6-gnome-2204-sdk"><img src="https://snapcraft.io/webkitgtk-6-gnome-2204-sdk/badge.svg" alt="WebKitGTK SDK Status"></a>
<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/update-sdk-snap.yml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/update-sdk-snap.yml/badge.svg"></a>

<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/release-sdk-to-candidate.yaml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/release-sdk-to-candidate.yml/badge.svg"></a>
<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/promote-to-stable.yml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/promote-to-stable.yml/badge.svg"></a>

</ul>

<b>WebKitGTK Content Snap Status</b>

<ul>
<a href="https://snapcraft.io/webkitgtk-6-gnome-2204"><img src="https://snapcraft.io/webkitgtk-6-gnome-2204/badge.svg" alt="WebKitGTK Content Snap Status"></a>
<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/update-sdk-snap.yml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/update-sdk-snap.yml/badge.svg"></a>
<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/release-content-to-candidate.yaml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/release-content-to-candidate.yml/badge.svg"></a>
<a href="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/promote-to-stable.yml"><img src="https://github.com/snapcrafters/webkitgtk-sdk/actions/workflows/promote-to-stable.yml/badge.svg"></a>
</ul>

## Uses

Using this SDK has two parts involved.

### Build

While building your app as a snap, you need to add these environment variables in the part that needs the webkitgtk-6+ libraries.

```yaml
build-environment:
  - PKG_CONFIG_PATH: /snap/webkitgtk-6-gnome-2204-sdk/current/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/pkgconfig${PKG_CONFIG_PATH:+:$PKG_CONFIG_PATH}
  - LD_LIBRARY_PATH: /snap/webkitgtk-6-gnome-2204-sdk/current/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  - PATH: /snap/webkitgtk-6-gnome-2204-sdk/current/usr/bin${PATH:+:$PATH}
```

### Runtime

To get the libraries at runtime, you need to connect the webkitgtk-6 content snap to your app snap.

For that you first need to define a plug and a layout for webkitgtk in the top level your `snapcraft.yaml`.

```yaml
layout:
  /usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/webkitgtk-6.0:
    bind: $SNAP/webkitgtk-platform/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/webkitgtk-6.0

plugs:
  webkitgtk-6-gnome-2204: # name of the plug
    interface: content # interface of snapcraft
    target: webkitgtk-platform # mount point of the content snap
    default-provider: webkitgtk-6-gnome-2204 # the default provider that'll provide this library
```

Now in the apps part which needs to use the webkitgtk-6+ libraries during runtime, you need to add the following environment variables.

```yaml
apps:
  app:
    environment:
      LD_LIBRARY_PATH: $SNAP/webkitgtk-platform/usr/lib:$SNAP/webkitgtk-platform/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR:$LD_LIBRARY_PATH
      PATH: $SNAP/webkitgtk-platform/usr/bin:$PATH
```

You can also declare these variables at the top level of your `snapcraft.yaml` if you want to use them in all the apps. Some snaps that use this webkitgtk-sdk.

- [foliate](https://github.com/johnfactotum/foliate/blob/gtk4/snapcraft.yaml)
- [planify](https://github.com/alainm23/planify/blob/master/snap/snapcraft.yaml)
- [bavarder](https://github.com/Bavarder/Bavarder/blob/main/snap/snapcraft.yaml)
- [imaginer](https://github.com/soumyaDghosh/Imaginer/blob/master/snap/snapcraft.yaml)
- [newsflash](https://github.com/soumyaDghosh/newsflash-snap/blob/main/snap/snapcraft.yaml)

And many others!

## How to contribute to this snap

Thanks for your interest! Below you find instructions to help you contribute to this snap.

The general workflow is to submit pull requests that merges your changes into the `2204` branch here on GitHub. Once the pull request has been merged, a GitHub action will automatically build the snap and publish it to the `candidate` channel in the Snap Store. Once the snap has been tested thoroughly, we promote it to the `stable` channel so all our users get it!

### Initial setup

If this is your first time contributing to this snap, you first need to set up your own fork of this repository.

1. [Fork the repository](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo) into your own GitHub namespace.
2. [Clone your fork](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository), so that you have it on your local computer.
3. Configure your local repo. To make things a bit more intuitive, we will rename your fork's remote to `myfork`, and add the snapcrafters repo as `snapcrafters`.

   ```shell
   git remote rename origin myfork
   git remote add snapcrafters https://github.com/snapcrafters/signal-desktop.git
   git fetch --all
   ```

### Submitting changes in a pull request

Once you're all setup for contributing, keep in mind that you want the git information to be all up-to-date. So if you haven't "fetched" all changes in a while, start with that:

```shell
git fetch --all -p
```

Now that your git metadata has been updated you are ready to create a bugfix branch, make your changes, and open a pull request.

1. All pull requests should go to the stable branch so create your branch as a copy of the stable branch:

   ```shell
   git checkout -b my-bugfix-branch snapcrafters/candidate
   ```

2. Make your desired changes and build a snap locally for testing:

   ```shell
   snapcraft --use-lxd
   ```

3. After you are happy with your changes, commit them and push them to your fork so they are available on GitHub:

   ```shell
   git commit -a
   git push -u myfork my-bugfix-branch
   ```

4. Then, [open up a pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) from your `my-bugfix-branch` to the `snapcrafters/candidate` branch.
5. Once you've opened the pull request, it will automatically trigger the build-test action that will launch a build of the snap. You can watch the progress of the snap build from your pull request (Show all checks -> Details). Once the snap build has completed, you can find the built snap (to test with) under "Artifacts".
6. Someone from the team will review the open pull request and either merge it or start a discussion with you with additional changes or clarification needed.
7. Once the pull request has been merged into the stable branch, a GitHub action will rebuild the snap using your changes and publish it to the [Snap Store](https://snapcraft.io/signal-desktop) into the `candidate` channel. After sufficient testing of the snap from the candidate channel, one of the maintainers or administrators will promote the snap to the stable branch in the Snap Store.

## Maintainers

- [@soumyaDghosh](https://github.com/soumyaDghosh/)
- [@merlijn-sebrechts](https://github.com/merlijn-sebrechts/)
- [@jnsgruk](https://github.com/jnsgruk/)

## License

- The licenses of WebkitGTK are GPL-2.0, GPL-3.0, LGPL-2.1, MIT and some other licenses. You can find more about the licenses in the [WebkitGTK source code](https://github.com/WebkitGTK/WebkitGTK?tab=License-1-ov-file). The license of this repository is MIT.
