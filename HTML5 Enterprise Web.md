HTML5 Enterprise Application Development
========================================

### create project structure
```bash
src/
src/jade
src/stylus
/dist
/dist/css
/dist/img
/dist/js
/dist/js/lib
```

### init project
```
npm init
npm install grunt --save-dev
npm install grunt-contrib-jade --save-dev
npm install grunt-contrib-stylus --save-dev
npm install grunt-contrib-watch --save-dev
npm install grunt-contrib-connect --save-dev
```

### create Gurntfile.js
```javascript
module.exports = function(grunt) {
  grunt.initConfig({
    stylus: {
      target: {
        expand: true,
        cwd: 'src/stylus',
        src: '**/*.styl',
        dest: 'dist/css',
        ext: '.css'
      }
    },

    jade: {
      options: {
        pretty: true
      },
      target: {
        expand: true,
        cwd: 'src/jade',
        src: '**/*.jade',
        dest: 'dist/',
        ext: '.html'
      }
    },

    connect: {
      options: {
        port: 3000,
        open: true,
        hostname: 'localhost',
        livereload: 35729,
        base: 'dist/'
      },
      livereload: {
      },
    },

    watch: {
      stylus: {
        files: 'src/stylus/**/*.styl',
        tasks: 'stylus'
      },
      jade: {
        files: 'src/jade/**/*.jade',
        tasks: 'jade'
      },
      livereload: {
        options: {
          livereload: 35729
        },
        files: [
          'dist/**/*.html',
          'src/stylus/**/*.styl',
          'src/jade/**/*.jade'
        ]
      }
    },
  });

  grunt.loadNpmTasks('grunt-contrib-stylus');
  grunt.loadNpmTasks('grunt-contrib-jade');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-connect');

  grunt.registerTask('default', ['connect', 'watch']);
};
```
### index.jade
```jade
doctype html
html(lang='zh')
  head
    title MovieNow
    meta(charset='utf-8')
    meta(name='description', content='Your movie theater finder')
    meta(name="author", content="Me")
    meta(name="keywords", content="movie, hollywood")
    meta(name="copyright", content="Copyright &copy;  2014 MovieNow. All rights reserved.")
    link(rel='shotcut icon', href='img/favicon.ico', type='image/x-icon')      
    link(rel='stylesheet', href='css/main.css')
    script(src='js/lib/modernizr.js')
    script(src='js/lib/jquery.min.js')
    script(src='js/lib/jquery.xdomainajax.js')
    script(src='js/movienow.js')
    script(src='js/movienow.geolocation.js')
  body
    section.wrapper
      .main
        header
          div MovieNow
        
        nav
          ul: li: a(href='index.html') Home
        
        aside
          h2 Top 5 Box Office
          ol
            li
              h3 Dark Knight Rises
              p Action
            li
              h3 Avengers
              p Action
            li
              h3 Ice Age: Continental Drift
              p Animation
            li
              h3 The Amazing Spider-Man
              p Action
            li
              h3 Dark Shadows
              p Comedy

        section
          article
            h1 Home Title
            p Home Content
            button#find-movies Find find-movies
            #movies-near-me

    footer Copyright &copy; 2014 MovieNow. All rights reserved.
```
### index.styl
```stylus
/*** RESET ***/
html
body
div
span
applet
object
iframe
h1
h2
h3
h4
h5
h6
p
blockquote
pre
a
abbr
acronym
address
big
cite
code
del
dfn
em
img
ins
kbd
q
s
samp
small
strike
strong
sub
sup
tt
var
b
u
i
center
dl
dt
dd
ol
ul
li
fieldset
form
label
legend
table
caption
tbody
tfoot
thead
tr
th
td
article
aside
canvas
details
figcaption
figure
footer
header
hgroup
menu
nav
section
summary
time
mark
audio
video
  margin 0
  padding 0
  border 0
  outline 0
  font-size 100%
  -webkit-text-size-adjust  100%
  font-weight inherit
  font-style inherit
  font-family inherit
  vertical-align baseline

table
  border-collapse collapse
  border-spacing 0

address
caption
cite
code
dfn
em
strong
th
var
  font-style normal
  font-weight normal

ol
ul 
  list-style none

article
aside
details
figcaption
figure
footer
header
hgroup
menu
nav
section
  display block

blockquote
q
  quotes none

blockquote:before
blockquote:after
q:before
q:after 
  content ''
  content none

:focus 
  outline:1px dotted #666
 

// general styles
html
body
.wrapper
  height 100%

.wrapper
  background #fff
  section
  nav
    padding 5px 35px

    

body
  font-family Helvetica, Arial, sans-serif
  width 960px
  margin 0 auto
  background #ccc

header
  color #fff
  height 122px
  background #1A1A1A url(../img/logo_back.png)
  div
    width 320px
    height 122px
    background url(../img/logo.png) no-repeat
    text-indent -100%

nav
  background-color #666
  padding 5px 35px
  a
    color #fff
    text-decoration none
    &:hover
      text-decoration underline

aside
  float right
  width 200px
  padding 30px 0 10px 0
  margin 0 10px
  background-color #E4E4E4
  h2
    padding 0 20px 10px
    margin 0 0 20px
    border-bottom 1px dashed #fff
    font-size 1.3em
  ol
    padding 0 0 0 36px
    counter-reset: counter;
    li
      color #666
      font-size .8em
      line-height 1.2em
      margin 0 0 8px 0
      position relative
      &:before
        counter-increment counter
        content counter(counter) ". "
        margin-right 5px
        color  #333
        position absolute
        top 0
        left -16px
      p
        color #999        

article h1
  font-size 1.5em
  margin 10px 0 5px

footer
  background-color #000
  color #fff
  font-size .6em
  line-height 2em
  text-align center
  width 960px
  margin-left auto
  margin-right auto
  position relative
  margin-top -2em

@media only screen and (min-width: 738px) and (max-width: 1024px)
  .wrapper
  footer
    width 100%

@media only screen and (max-width: 737px)
  aside
    display none
  
  .wrapper
  footer
    width 100%
  
  .wrapper section
  nav
    padding 5px 15px
  
  header div
    background-position  -50px 0px
```






