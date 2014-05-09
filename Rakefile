# MICS Report - Rakefile

task :init do
  `mkdir src`
  `mkdir config`
  `mkdir tmp`
  `mkdir output`

  `touch config/tex_files`
end

task :reset do
  `rm -rf src`
  `rm -rf config`
  `rm -rf tmp`
  `rm -rf output`
end

task :create do
  if ENV['name'].nil?
    puts "ERROR: NO NAME"
    puts "execute `rake create name=NAME`"
    exit
  end
  `touch src/#{ENV['name']}.tex`
  `echo #{ENV['name']}.tex >> config/tex_files`
end

