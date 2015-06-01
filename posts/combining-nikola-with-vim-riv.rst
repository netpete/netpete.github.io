.. title: Combining Nikola with Vim Riv
.. slug: combining-nikola-with-vim-riv
.. date: 2015-03-26 08:22:40 UTC-07:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

I use Riv_ (RestructuredText in Vim) for all of my note taking.  I especially use the Scratch feature to create automatically time stamped entries.  The main reason I use Riv is that I can have all of my notes in a repository (Mercurial or Git).   Riv bills itself as working well with Nikola and Pelican so I decided I would try it out.

My Goal was to continue to use RIV for my note taking and use Nikola_ to render the site to share with teammates.

I wanted to see how well these tools can be integrated with out modifying the code of either one.

I was able to initialize a Nikola site into my Riv Project directory.  The first thing I attempted was to simply add the Scratch directory to the posts. 

.. code:: python

    POSTS = (
        ("posts/*.rst", "blog", "post.tmpl"),
        ("posts/*.txt", "blog", "post.tmpl"),
        ("Scratch/*.rst", "scratch", "post.tmpl"),
    )

This would have been an ideal situation as the Scratch directory is date based and I could have an RSS feed attached to it.  This did not work because there is not enough meta data in the Scratch directory for Nikola to create posts from them.  I may come back to modifying the CreateScratch function in Riv to automatically generate posts if I need to modify code.

The next thing I tried was to create a Nikola site with pages instead of the posts especially since Riv already automatically creates an index.rst file of the Scratch directory.  Setting the Scratch directory to a pages and then adding that as a section on the menu worked great. 

.. code:: python

    PAGES = (
        ("stories/*.rst", "stories", "story.tmpl"),
        ("stories/*.txt", "stories", "story.tmpl"),
        ("Scratch/*.rst", "scratch", "story.tmpl"),
    )

    # ...

    NAVIGATION_LINKS = {
        DEFAULT_LANG: (
            ("/scratch/", "Notes"),
            ("/categories/", "Tags"),
            ("/rss.xml", "RSS feed"),
        ),
    }


Riv allows for a project index file that is the first file that opens the when the project is opened.  The next thing I waned to do was have Nikola pick this up as the index page.  This is where I ran in to the configuration limits.  Nikola wants all files to be in a subdirectory of the directory containing the conf.py file and by default Riv puts this at the top level.  Riv allows a master_doc option to allow for the master doc to be at another level, but the master_doc option affects all directories including the scratch directory which stopped the Scratch pages.  After a nights sleep I realized that the Riv project did not need to be at the root of Repository.  By changing the Riv project directory to the 'src' directory in my vimrc file I was able to get Riv and Nikola to both interact with my notes without modifying the code of either project.

My vimrc file looks like this

.. code:: vim

  "Riv Settings
  let g:riv_todo_keywords = "TODO,DONE;FIXME,FIXED;START,PROCESS,STOP;AR,DONE"
  let g:riv_debug=1
  let g:riv_file_link_style = 2
  let pgtiegs_notes = {'Name':"Peter's Notes", 'path': '~/notes/src',}
  let g:riv_projects = [pgtiegs_notes] 

My Nikola conf.py looks like this

.. code:: python

    PAGES = (
        ("src/index.rst", "", "story.tmpl"),
        ("stories/*.rst", "stories", "story.tmpl"),
        ("stories/*.txt", "stories", "story.tmpl"),
        ("src/Scratch/*.rst", "scratch", "story.tmpl"),
    )

.. _Nikola : http://getnikola.com
.. _Riv : https://github.com/Rykka/riv.vim



