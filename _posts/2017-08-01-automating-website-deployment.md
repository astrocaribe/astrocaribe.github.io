---
layout: post
title:  "Automating Website Deployment"
date:   2017-08-01 22:46:00
author: astrocaribe
category: dev
tags: [website gulp]
---

### TL;DR:

I use Grunt to deploy my updated site, with the configured tasks:

```
grunt sftp
```

All my files get pushed to the host, updating the site once it is done.

### The Journey To Recollection

My current résumé-style website, [AstroCaribe.com][my-website], is a hosted site
that I will occasionally need to update (ok, I'm a few months behind, alright?).
The problem is that, lately, I have been doing it so infrequently, that I forget
how to deploy changes to my site.

Notes, you say? Notes are for suckers! But I suffer because of that thought
process. Again, this is what this blog is for, a digital notebook!

### Strolling Down Memory Lane...

After digging around my site for a little, I remembered that I am using
[Grunt][grunt-site], and I have a `Gruntfile.js` configuration all ready to go.
Oh, look! I've set up grunt tasks that will basically push my updated site to
the host, with minimal effort! Here's what the relevant part of mine looks like:

{% highlight javascript %}
module.exports = function(grunt) {
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),

    // unrelated tasks
    ...

    // host options for grunt-ssh tasks
    sshconfig: {
      "myhost" : grunt.file.readJSON('secret.json')
    },

    // grunt-ssh sftp task
    sftp: {
      upload: {
        files: {
          "./": ["assets/**", "css/\**", "favicon.ico", "fonts/**", "js/*", "index.html"]
        },
        options: {
          path:              '<%= sshconfig.myhost.path %>',
          config:            'myhost',
          showProgress:      true,
          createDirectories: true
        }
      }
    },
  });

  grunt.loadNpmTasks('grunt-ssh');
  grunt.registerTask('default',['watch']);
};
{% endhighlight %}

### Dev Dependencies:
`grunt-ssh`: Used to upload the site via ssh w/ sftp

### Just A Little Insight

For those familiar with Grunt, the above shouldn't be to tough. For the
uninitiated, I highly recommend [reading the docs][grunt-docs]. I'll point out
the pertinent parts below:

`sshconfig`: Contains the host ssh credentials to that it can connect and copy
the site files via sftp.

`sftp`: The Grunt task that performs the site files upload. it uses the config
read in `sshconfig`, including ssh url and path of the hosted site. I want to
`showProgress` (I want o know how long this take, you know!), as well as
`createDirectories`, if there are new ones.

That's it! Now that it's all setup, and I am content with the changes to the
site, I initial the upload Grunt task:

```
grunt sftp
```

Seeing the following confirms that I did something right:
```
[astrocaribe:~/projects/some-site] grunt sftp
Running "sftp:upload" (sftp) task
assets/docs/tleblanc_resume.pdf [====================] 100% of 1MB
assets/img/Self_obs.png [====================] 100% of 153KB
assets/img/fisk.jpg [====================] 100% of 40KB
assets/img/placeholder_140x140.gif [====================] 100% of 485B
assets/img/portrait-md-rnd-1.jpg [====================] 100% of 48KB
assets/img/portrait-md-rnd.jpg [====================] 100% of 34KB
assets/img/umet.jpg [====================] 100% of 718KB
assets/img/vanderbilt.png [====================] 100% of 14KB
assets/js/ie-emulation-modes-warning.js [====================] 100% of 2KB
assets/js/ie10-viewport-bug-workaround.js [====================] 100% of 641B
...
```

### Caveat
One aspect of this task that I'm not completely happy with, is that it uploads
*all* my files; wether they were updated or not. Currently, this isn't a big
issue, since the site is so small. At most, I loose 1-2 minutes in the upload,
which isn't a worry right now.

### The end
I know there are better ways to manage website deployments, but this is one I
found on my own, and will eventually upgrade to a better method once I have the
chance. How would you deploy your site?


[my-website]: http://astrocaribe.com
[grunt-site]: https://gruntjs.com/
[grunt-docs]: https://gruntjs.com/getting-started
