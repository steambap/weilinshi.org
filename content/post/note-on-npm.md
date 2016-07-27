+++
date = "2016-04-07T11:51:51+08:00"
draft = false
title = "note on npm"
image = "no-img2.png"

+++

## note on npm module

use a module that has a dependency on a git repository is a *bad* idea.  
Because the git repository may change in an unexpected way.  
Even if you lock the module's npm version, it will not help.  

I have a project that has a dependency on a qrcode module, which in turn has
a dependency on a git repository. When my colleague pull my code and npm install
, npm failed with a strange git error. The module did not update and we have the
same version of the module, but the git repository updated so the module somehow
cannot install properly. The only fix was that we report an issue to the author
and wait for update. There's no rollback. We have to stop coding for the day.
  
Unless you have to, don't use some lesser known module that depend on a git repository.
