+++
date = "2016-04-07T11:51:51+08:00"
draft = false
title = "note on npm"
image = "no-img2.png"

+++

## note on npm module

### 1.Be careful with git dependency
use a module that has a dependency on a git repository is a **_BAD_** idea.  
For normal npm modules, you can lock its version or generate shrink wrap to make sure everything is exactly the same.  
However, those does not apply to git module.  

I have a project that has a dependency on a qrcode module, which in turn has
a dependency on a git repository. When my colleague pull my code and npm install
, npm failed with a strange git error. 

Clearly, it's the git repository that causes the trouble. The only fix was that we report an issue to the author
and wait for update. There's no rollback. 
  
Unless you have to, don't use some lesser known module that depend on a git repository. If the git repository update, it will surprise you with npm install fail.

### 2.Todo
Sorry, there's nothing here. It's a placeholder :).
