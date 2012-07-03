watcher = require("tree-watcher")
exec = require('child_process').exec
fs = require("fs")
color = require("ansi-color").set

task "watch:l", "build Less", (options) ->
  OUTOUT_DIR = "public/styles"
  SRC_DIR = "app/styles/"
  
  lesswatcher = new watcher.Watcher 
    filter: (filename) ->
        filename.toLowerCase().indexOf "less" != -1
    throttle: 10
      
  lesswatcher.on "change", (event, path, watcher)->
    pattern  = ///
      app/
      /? #Trailing slash on output dir
      ([a-z]+) # First dir
      (?:/[a-z]*)? #Subdirs if any
      /([a-z]+) #filename
      .([a-z]+) #extension
    ///
    normalPath = escape(path).replace(/%5C/g, "/")

    x =  pattern.exec(normalPath)
    try
      filename = pattern.exec(normalPath)[2]
    catch e
      console.log "Less error", e

     exec "lesscn #{SRC_DIR}main.less #{OUTOUT_DIR}/main.css -x", (err, stdout, stderr) ->
        if err
          console.log "ERROR: #{err} || #{stdout} || #{stderr}"
        else
          console.log "less: compiling #{path}|| #{stdout} || #{stderr}"
    
  lesswatcher.watch SRC_DIR, (err, watcher)->
    if err
        console.log "ERROR: " + err
    else 
        console.log("Sass: Watching coffee-script files in #{SRC_DIR}")

task "watch:s", "build SASS files",(options) ->
  OUTOUT_DIR = "public"
  SRC_DIR = "app/styles/"
  
  sassWatcher = new watcher.Watcher 
    filter: (filename) ->
        filename.toLowerCase().indexOf "scss" != -1
    throttle: 10
      
  sassWatcher.on "change", (event, path, watcher)->
     exec "compass compile --css-dir #{OUTOUT_DIR} --sass-dir #{SRC_DIR}", (err, stdout, stderr) ->
        if err
          console.log "ERROR: #{err} || #{stdout} || #{stderr}"
        else
          console.log "compass: compiling #{path}|| #{stdout} || #{stderr}"
    
  sassWatcher.watch SRC_DIR, (err, watcher)->
    if err
        console.log "ERROR: " + err
    else 
        console.log("Sass: Watching coffee-script files in #{SRC_DIR}")

