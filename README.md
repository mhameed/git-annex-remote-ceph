# git annex remote ceph 0.1.0 #

Special remote program for git-annex to use a Ceph pool as a backend.
For more info see: http://git-annex.branchable.com/special_remotes/external/

# Requirements:

    python2
    python-ceph

# Install
Clone the git repository to your home folder.

    git clone git://github.com/mhameed/git-annex-remote-ceph.git 

This should make a ~/git-annex-remote-ceph folder.

# Setup
Make the file executable, and link it into PATH

    cd ~/git-annex-remote-ceph; chmod +x git-annex-remote-ceph; sudo ln -sf `pwd`/git-annex-remote-ceph /usr/local/bin/git-annex-remote-ceph

# Commands for git-annex:

    git annex initremote my-ceph type=external externaltype=ceph encryption=none rados_id=user-id-01 pool_name=test02 ceph_conffile=/etc/ceph/ceph.conf    
    git annex describe my-ceph "data on ceph pool test02"

If you want to store independant annex trees in the same ceph pool it is advisable to make use of the optional 
`key_prefix` (which defaults to `annex-`). Otherwise one git annex repo might
deduce that the additional keys that it is not aware of is unused keys, and are
droppable, but infact they belong to the other git annex repo.

    $ git annex initremote my-ceph type=external externaltype=ceph key_prefix=podcastannex- ...
    $ rados -p test02 ls
    annex-SHA256E-...
    ...
    podcastannex-SHA256E-...
    ...
    $

To enable debugging:

    $ export GIT_ANNEX_REMOTE_CEPH_DEBUG=1
    $ git annex get test.mp3
    ...
    $ cat /tmp/git-annex-remote-ceph.log

