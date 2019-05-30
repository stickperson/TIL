# Auto-reload in iPython

I've been meaning to do this for literally years.

Create an ipython profile if you do not already have one.

```
ipython profile create
```

Edit your profile (on osx it's at `~/.ipython/profile_default/ipython_config.py`):

```
# These two lines will probably be commented out somewhere
c.InteractiveShellApp.exec_lines = ['%autoreload 2']
c.InteractiveShellApp.extensions = ['autoreload']
```

Say goodbye to exiting/restarting iPython every time your code changes.