.. vim: filetype=rst :

sfawesome
=========

SF awesome is a command-line interface to sales force.

There is a secondary component to sfawesome which supports a kind of
'git commit' integration with salesforce.  When enabled, a commit
message starting with 'SF:XXXXXXXX' -- where XXXXXXXX is a salesforce
case -- will be added as a case comment in salesforce for that case.

Installation
------------

Run the install.bash script to install it::

    $ cd sfawesome

    $ ./install.bash <path-to-git-hooks-dir>

Basic Commands
--------------

These are the main command flags you can pass to the application:

    --create ::

        Creates a new case in salesforce

    --update-case ::

        Updates a field or fields in a case with new information.  The
        fields are specified through optional command line arguments.

    --get-ids ::

        Prints out a formatted list of cases and ids in salesforce.  You
        can narrow the results via optional command line arguments

    --get-comments ::

        Prints out the comments associated with a case.  The results can
        be ordered by field and the results can be narrowed down via
        optional command line arguments.

Each of the main commands has a set of optional flags/options/sub
commands associated with it that can alter the way the main command
functions.  See below.

Update Case
---------------------------
--update-case <Case Number>

optional flags/options::

    --status=<Status>

    --owner=<Owner>

    --release=<Release>

    --type=<Type>

    --priority=<Priority>

examples::

    $ git-sf --update-case --status=New --owner=Evan 00001185

    $ git-sf --update-case --type=Feature --release=Ogre 00001185

Add Note
------------------------
--add-note <Case Number>

optional flags/options::

    None

examples::

    $ git-sf --add-note="This is a note" 00001185


Get Ids (List Cases)
--------------------
--get-ids=<Release>

optional flags/options::

    --order=<Field To Order On>

    --reverse

    --owner_list=<Owner_1[,Owner_2]>

    --grep=<SQL Search String>

examples::

    $ git-sf --get-ids --release=nosehair --order=Date --reverse

    $ git-sf --get-ids --order=Developer --grep="overtime"


Get Case Comments
----------------------------
--get-comments <Case Number>

optional flags/options::

    --order=<Field To Order On>

    --reverse

    --grep=<SQL Search String>

examples::

    $ git-sf --get-comments --order=Date --grep="Testing" 00001185

    $ git-sf --get-comments --order=Status --reverse 00001185


Create A Case
-------------
--create=<Subject>

optional flags/options::

    --owner=<Owner>

    --release=<Release>

    --description=<Description>

    --status=<Status>

    --priority=<Priority>

    --type=<Type>

examples::

    $ git-sf --create="This is a new case"

    $ git-sf --create="This is a new case" --release=Ogre \
      --priority="High"

    $ git-sf --create="This is a new case" --type="Bug" --owner="Evan"


Configuration File Options
--------------------------
username::
    The username for salesforce

password::
    The password for salesforce

token::
    The token for access to the API

owners::
    A python list of valid owners for cases

releases::
    A python list of valid release names

statuses::
    A python list of valid statuses for cases

priorities::
    A python list of valid priorities for cases

types::
    A python list of valid types of cases


Examples
--------

Show all cases in the nosehair release::

    $ git-sf --get-ids --release=nosehair --order-by=Status

Show just one person's cases in the nosehair release::

    $ git-sf --owner=Matt --get-ids=Nosehair

Show people's cases in the nosehair release::

    $ git-sf --owner=Matt,Tim --get-ids=Nosehair

Close a case::

    $ git-sf --update-status=Closed 00001185

Show all comments for a case ordered by CreatedDate and reversed::

    $ git-sf --get-comments --order=Date --reverse 00001182

See the description for a case::

    $ git-sf --get-details 00001182

Create a case::

    $ git-sf --create="This is a case subject"

Create a case with all the options::

    $ git-sf --create="Subject" --description="Description" --release="release" --type="Bug" --status="Working" --owner="Evan" --Priority="High"

    NOTE: By default you only need to provide the subject.
          Everything else will have a default values of:
            Description = ""
            Release = ""
            Type = ""
            Status = "New"
            Owner = ""
            Priority = "Low"

Update a case one field at a time::

    $ git-sf --update-case --owner=Evan 00001182

    $ git-sf --update-case --status=Working 00001182

    $ git-sf --update-case --release=mollusk 00001182

    $ git-sf --update-case --priority=Medium 00001182

    $ git-sf --update-case --type=Bug 00001182

Update several fields in a case at once::

    $ git-sf --update-case --owner=Evan --status=Working --release=mollusk 00001182

git integration examples
========================

Add a note to the case during a git commit::

    $ git commit -a -m "SF:00001185 your momma don't wear no drawers"

