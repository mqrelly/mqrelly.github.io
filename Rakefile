
desc "Start up WEBrick server"
task :view do
  exec "bundle exec nanoc view"
end

desc "Watch files for changes and rerun nanoc compile if needed"
task :build do
  exec "filewatcher --dontwait 'Rules layouts content' 'bundle exec nanoc compile --warn'"
end

desc "Compile and check the site then publish to origin (assumed GitHub Pages)"
task :publish do
  uncommitted_files = `git status --short`
  fail "ERROR: There are uncommitted changes to files\n#{uncommitted_files}" unless uncommitted_files.strip.empty?

  puts `bundle exec nanoc prune --yes`
  puts `bundle exec nanoc compile --warn`
  # TODO: Run all nanoc checks

  last_publish_date = `git log -1 --format=%aD gh-pages`
  changes_since_last_publish = `git log --since="#{last_publish_date}" --format=%s`
  changes_since_last_publish = changes_since_last_publish
    .split("\n")
    .map { |line| "- #{line}" }
    .join("\n")

  in_site_dir do
    puts `git add -A`
    puts `git commit --message='Published on #{Time.now.utc}\n\n#{changes_since_last_publish}'`
  end

  puts `git checkout gh-pages`
  puts `git pull site`
  puts `git checkout master`

  puts `git push origin master:source gh-pages:master`
end


def in_site_dir
  Dir.chdir "_site"
  begin
    yield
  ensure
    Dir.chdir ".."
  end
end
