# MICS Report - Rakefile
require 'yaml'

SRC = "tex"
TMP = "tmp"
OUTPUT = "output"
CONFIG = "config"
LIB = "lib"
CODE = "src"
IMAGES = "images"

# init / reset project files
task :init do
  `mkdir #{SRC}`
  `mkdir #{TMP}`
  `mkdir #{OUTPUT}`
  `mkdir #{CONFIG}`
  `mkdir #{CODE}`
  `mkdir #{IMAGES}`

  `cp #{LIB}/title.yml.sample #{CONFIG}/title.yml`

  `touch #{CONFIG}/tex_files`
  `touch #{TMP}/title.tex`
  `touch #{TMP}/body.tex`
  `touch #{TMP}/footer.tex`
  `echo "\\end{document}" > #{TMP}/footer.tex`

  `git remote remove origin`
end

task :reset do
  `rm -rf #{SRC}`
  `rm -rf #{TMP}`
  `rm -rf #{OUTPUT}`
  `rm -rf #{CONFIG}`
  `rm -rf #{CODE}`
  `rm -rf #{IMAGES}`
end

# create new .tex file
task :create do
  if ENV['name'].nil?
    puts "ERROR: NO NAME"
    puts "execute `rake create name=NAME`"
    exit
  end

  `touch #{SRC}/#{ENV['name']}.tex`
  `echo #{ENV['name']}.tex >> #{CONFIG}/tex_files`
end

task :make_title do
  title = YAML.load_file("#{CONFIG}/title.yml")
  `cat #{LIB}/title.tex > #{TMP}/title.tex`
  `echo '\\\\title{#{title['title']}}' >> #{TMP}/title.tex`
  `echo '\\\\author{#{title['name']}}' >> #{TMP}/title.tex`
  `echo '\\\\\date{\\\\today}' >> #{TMP}/title.tex`
  `echo '\\\\id{#{title['stdid']}}' >> #{TMP}/title.tex`
  `echo '\\\\course{#{title['course']}}' >> #{TMP}/title.tex`
  `echo '\\\\begin{document}' >> #{TMP}/title.tex`
  `echo '\\\\maketitle' >> #{TMP}/title.tex`
  `echo '\\\\clearpage' >> #{TMP}/title.tex`
end

task :make_body do
  cmd = ["cat"]
  open "#{CONFIG}/tex_files" do |file|
    while l = file.gets
      cmd << "#{SRC}/#{l.chomp}"
    end
  end

  `#{cmd.join(" ")} > #{TMP}/body.tex` if cmd.size > 1
  `cp #{CODE}/* #{TMP}/`

  `cp #{IMAGES}/*.png #{TMP}/`
  `cd #{TMP} && extractbb *.png`
end

task :concat do
  `cat #{TMP}/title.tex #{TMP}/body.tex #{TMP}/footer.tex > #{TMP}/report.tex`
end

task :make_pdf do
  `cd #{TMP} && platex report.tex && platex report.tex`
  `cd #{TMP} && dvipdfmx report.dvi`
  `mv #{TMP}/report.pdf #{OUTPUT}/report.pdf`
end

task :compile => [:make_title, :make_body, :concat, :make_pdf]

task :default => :compile
