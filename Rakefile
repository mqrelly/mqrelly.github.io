
desc "Start up WEBrick server"
task :view do
  exec "bundle exec nanoc view"
end

desc "Watch files for changes and rerun nanoc compile if needed"
task :build do
  exec "filewatcher --dontwait 'Rules layouts content' 'bundle exec nanoc compile --warn'"
end

