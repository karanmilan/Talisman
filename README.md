This tool is forked from the original tool present on https://github.com/thoughtworks/talisman, small changes have been made to support the pre-commit hook.

# Talisman

Talisman is a tool to validate code changes that are to be pushed out
of a local Git repository on a developer's workstation. By hooking
into the pre-push hook provided by Git, it validates the outgoing
changeset for things that look suspicious - such as potential SSH
keys, authorization tokens, private keys etc.

The aim is for this tool to do this through a variety of means
including file names and file content. We hope to have it be an
effective check to prevent potentially harmful security mistakes from
happening due to secrets which get accidentally checked in to a
repository.

The implementation as it stands is very bare bones and only has the
skeleton structure required to add the full range of functionality we
wish to incorporate. However, we encourage folks that want to
contribute to have a look around and contribute ideas/suggestions or
ideally, code that implements your ideas and suggestions!

#### Running Talisman

Talisman can either be installed into a single git repo, or as a
[git hook template](https://git-scm.com/docs/git-init#_template_directory), which would let Talisman to be present in any new repository that you 'init' or 'clone' and install it to existing repositories of a parent directory.

We recommend installing it as a git hook template, as that will cause
Talisman to be present in any new repository that you 'init' or
'clone'.

To have Talisman present for all your git repositories of your local machine, run it from your root directory.

You could download the
[Talisman binary](https://github.com/karanmilan/talisman/releases)
manually and copy it into your project/template `hooks` directory --
or you can use our `install.sh` script.

Linux and Mac users can use the instructions below
```bash
curl https://raw.githubusercontent.com/karanmilan/talisman/master/install.sh > ~/install-talisman.sh
chmod +x ~/install-talisman.sh
```
Windows users can use these instructions below
```bash
curl https://raw.githubusercontent.com/karanmilan/talisman/master/install-talisman-windows.sh > ~/install-talisman.sh
```

If you run this script from inside a git repo, it will add Talisman to
that repo. Otherwise, it will prompt you to install as a git hook
template.

```bash
# Install to a single project
cd my-git-project
~/install-talisman.sh hook-type  (hook type could be pre-push or pre-commit)
```

```bash
# Install as a git hook template
cd ~
~/install-talisman.sh hook-type (hook type could be pre-push or pre-commit)
```

```bash
# Install into all child directories of a parent Directory, would appear only if also installed as git-template
Directory Structure
--> Parent-directory
	|
	|
	+-- Child-directory-1
	|
	|
	+-- Child-directory-2
	|
	|
	+-- Child-directory-3

cd Parent-directory
~/install-talisman.sh hook-type

From now on Talisman will run checks for obvious secrets automatically before each push or commit depending on the hook provided to the installation script or binary copied:

```bash
$ git push
The following errors were detected in danger.pem
         The file name "danger.pem" failed checks against the pattern ^.+\.pem$

error: failed to push some refs to 'git@github.com:jacksingleton/talisman-demo.git'
```

#### Ignoring Files

If you're *really* sure you want to push that file, you can add it to
a `.talismanignore` file in the project root:

```bash
echo './danger.pem' >> .talismanignore
```

Note that we can ignore files in a few different ways:

* If the pattern ends in a path separator, then all files inside a
  directory with that name are matched. However, files with that name
  itself will not be matched.
  
* If a pattern contains the path separator in any other location, the
  match works according to the pattern logic of the default golang
  glob mechanism.
  
* If there is no path separator anywhere in the pattern, the pattern
  is matched against the base name of the file. Thus, the pattern will
  match files with that name anywhere in the repository.

#### Developing locally

To contribute to Talisman, you need a working golang development
environment. Check [this link](https://golang.org/doc/install) to help
you get started with that.

Once that is done, you will need to have the godep dependency manager
installed. To install godep, you will need to fetch it from Github.

```` go get github.com/tools/godep ````

Once you have godep installed, clone the talisman repository. In your
working copy, fetch the dependencies by having godep fetch them for
you.

```` godep restore ````

To run tests ```` godep go test ./...  ````

To build Talisman, we can use [gox](https://github.com/mitchellh/gox):

```` gox -osarch="darwin/amd64 linux/386 linux/amd64" ````

#### Contributing to Talisman

TODO: Add notes about forking and golang import mechanisms to warn
users.
