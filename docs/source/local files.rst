Local Files Setup
=================

* Checkout avatar-server-pack repo on GitHub (https://github.com/ei8/avatar-server-pack.git)
* Overwrite existing database files with blank databases

  * Go to ``avatar-server-pack\avatars\prod\blank``
  * Copy all ``.db`` files
  * Paste all to ``avatar-server-pack\avatars\prod\sample``

* Open ``.env`` file and modify
* Run Docker Compose

  * ``docker-compose -f docker-compose.yml up``
