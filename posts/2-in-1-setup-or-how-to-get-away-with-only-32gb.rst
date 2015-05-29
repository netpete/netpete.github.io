.. title: 2 in 1 Setup or How to get away with only 32GB
.. slug: 2-in-1-setup-or-how-to-get-away-with-only-32gb
.. date: 2015-05-27 10:35:51 UTC-07:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text 

I recently decided to pick up a Windows machine and I really liked the idea of having a `2 in 1`_.  So I did my research and purchased an Acer_ Switch 11.  Overall I have been very happy with this little laptop/tablet.  Knowing that I would be using this for development I got the model with a 500GB HD in the keyboard.  Unfortunately this model only came with 32GB on the primary disk.

My end goal was to be able to install `Visual Studio Community Edition`_ on this machine for use in the Laptop mode, but not necessarily in tablet mode.
    
The first thing I tried to do was to remove the recovery partition.  For some reason the OEM version of Windows provided with the Acer Switch did not allow for that.  Every time I tried to remove the partition it would prevent the OS from booting.  A quick Google search showed that this was a fairly common problem.  Fortunately I had a retail version Windows, so I hooked up a USB DVD drive and started from scratch.  I was able to create single partition for the OS and start the install.  Everything worked great...

...Almost, when the retail version of Windows booted the touch screen was no longer working.  So back to the Acer site and a quick download and install of the Intel platform drivers got everything back to working.  I still did not have Visual Studio installed.  

My first attempted at installing Visual Studio to the D: drive failed.  While the primary interface was installed on the D: drive the bulk of the other content is still installed on the C: drive.  So I still need more space.  Next I added in a 64GB micro SD card and Robocopy'd my users directory to it.  To do this I created a local user with admin privileges and logged in as him. Then I removed the original home directory, and created a link to the home directory on the F: drive.

.. code::

        robocopy /COPYALL /MIR /XJ c:\Users\Peter f:\Users\Peter
        rd /S /Q c:\Users\Peter
        mklink /J c:\Users\Peter f:\Users\Peter


When that was done I logged back in as myself, removed the local user and everything is working again.  But I still don't have a working version of Visual Studio.  I needed more space.  Then I downloaded WinDirStat_ to analyze what was eating the most space.  It was the Visual Studio Package Cache in the ProgramData directory.  I had read that this was not something that should be removed and that it was needed for Visual Studio to work.  So I applied the same process that I used with my home directory to the package cache.  Robocopy the files to the D: drive, remove the files from the C: drive and link the D: drive files back to C: drive.  Now I was able to repair the installation of Visual Studio and fire it up.

I did this process two more times with the Windows SDKs and Microsoft Kits and now I have a healthy 3GB of the 32GB on the C: drive my users directory has over 50GB left on in and Visual Studio work when I am docked with the Keyboard and actually need it.

I was always an extensive user of the :code:`ln -s` soft links in Posix operating system.  I am not sure when :code:`mklink` got added to the OS but I really appreciate the capabilities it provides.

.. _WinDirStat: http://windirstat.info/ 
.. _`Visual Studio Community Edition`: http://www.visualstudio.com
.. _`2 in 1`: http://www.intel.com/content/www/us/en/homepage.html
.. _Acer : http://us.acer.com/ac/en/US/content/series/aspireswitch11
