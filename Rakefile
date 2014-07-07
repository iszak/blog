require "rubygems"
require "rake"

require "bundler/setup"
require "jekyll"

namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "gh-pages"
    })).process
  end

  desc "Commit the local site to the gh-pages branch and publish to GitHub Pages"
  task :publish do
    # Ensure the gh-pages dir exists so we can generate into it.
    puts "Checking for gh-pages dir..."
    unless File.exist?("./gh-pages")
      puts "No gh-pages directory found. Run the following commands first:"
      puts "  `git clone git@github.com:iszak/blog.git gh-pages"
      puts "  `cd gh-pages"
      puts "  `git checkout gh-pages`"
      exit(1)
    end

    # Ensure gh-pages branch is up to date.
    Dir.chdir('gh-pages') do
      sh "git pull origin gh-pages"
    end

    # Copy to gh-pages dir.
    puts "Copying site to gh-pages branch..."
    Dir.glob("*") do |path|
      next if path.include? "gh-pages"
      sh "cp -R #{path} gh-pages/"
    end

    # Commit and push.
    puts "Committing and pushing to GitHub Pages..."
    sha = `git log`.match(/[a-z0-9]{40}/)[0]
    Dir.chdir('gh-pages') do
      sh "git add ."
      sh "git commit --allow-empty -m 'Updating to #{sha}.'"
      sh "git push origin gh-pages"
    end
    puts 'Done.'
  end
end
