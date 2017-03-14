---
title: Set Up GitHub Pages
---

### GitHub::Pages::CreateSite()

```ruby
# Create a repo for github pages
newRepo = GetHub::Repository.new "log"
newRepo.Settings.Pages.Theme = "minimal"
newRepo.Settings.Pages.Source = "master -- /"
newRepo.Settings.Pages.Enabled = true

# Clone repo
term = LocalHost.Term.new()
term.exec("git clone https://github.com/ch1r0/log.git")
term.exec("git config user.name ch1r0")
term.exec("git config user.email <email>")

# Hello Pages
term.exec("cd log")
term.exec("echo # Crunch Time > index.md")
term.exec("git add index.md")
term.exec("git commit -m \"index.md\"")
term.exec("git push")

# Exclude
vimCfg = term.exec("vim _config.yml")
vimCfg.i("exclude:  [README.md, CNAME]")
vimCfg:x()
term.exec("git commit -a -m \"Exclude README.md and CNAME\"")
term.exec("git push")

term.exec("cd ..")
```

### Jekyll::LocalSetup()
```ruby
# Generate default Gemfile
term = LocalHost.Term.new()
term.exec("sudo gem install github-pages")
term.exec("jekyll new temp")
term.exec("cp temp/Gemfile log/Gemfile")
term.exec("rm -rf temp")
term.exec("cd log")

# Remove jekyll, add github-pages in Gemfile
vimGem = term.exec("vim Gemfile")
vimGem./("^gem \"jekyll\"")
vimGem.0()
vimGem.i("# ")
vimGem./("^# gem \"github-pages\"")
vimGem.0()
vimGem.x(2)
vimGem:x()

# Git Ignore
vimIgn = term.exec("vim .gitignore")
vimIgn:$()
vimIgn.a(".*")
vimIgn.a("Gemfile")
vimIgn.a("Gemfile.lock")
vimIgn.a("Rakefiles")
vimIgn.a("_site")

# Start local jekyll server
term.exec("bundle exec jekyll serv")
```
