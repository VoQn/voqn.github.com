task :default => :preview

desc 'Launch preview jekyll'
task :preview => [:assets, :quickrun]

desc 'Pushing repository to Github'
task :release do
  message = "Site updated at #{Time.now.strftime '%Y-%m-%d'}"
  sh "git add ."
  sh "git commit -m \"#{message}\""
  sh "git push origin master"
end

vender_dir = './_vender'
temp_dir = './_temp'
script_dir = './js'

script_deps = {
  'javascript' => [
    'jquery-1.7.2',
    'handlebars-1.0.0.beta.6'
  ],
  'coffee' => [
    'handlebars-helper',
    'jquery-handlebars',
    'config',
    'main',
    'handlebars-github-activity'
  ]
}

desc 'Generate frontend javascript file'
task :make_script do
  js_files = script_deps['javascript'].map do |n|
    "#{vender_dir}/js/#{n}.js"
  end
  coffee_files = script_deps['coffee'].map do |n|
    "#{vender_dir}/coffee/#{n}.coffee"
  end
  sh "cat #{js_files.join ' '} > #{temp_dir}/lib.js"
  sh "cat #{coffee_files.join ' '} > #{temp_dir}/user_script.coffee"
  sh "coffee -b -c #{temp_dir}/user_script.coffee"
  sh "cat #{temp_dir}/lib.js #{temp_dir}/user_script.js > #{temp_dir}/script.js"
  sh "uglifyjs -nc -o #{script_dir}/script.min.js #{temp_dir}/script.js"
end

desc 'Compile and Compress LessCSS'
task :make_stylesheet do
  sh "lessc #{vender_dir}/less/style.less > #{temp_dir}/style.css"
  sh "uglifycss #{temp_dir}/style.css > ./css/style.css"
end

desc 'Clean temporary directory'
task :clean do
  sh "rm -rf #{temp_dir}"
end

desc 'Prepare temporary directory'
task :prepare do
  sh "mkdir #{temp_dir}"
end

desc 'Compile and distination *.coffee & *.less'
task :assets => [:clean, :prepare, :make_script, :make_stylesheet]

desc 'Launch preview jekyll server'
task :quickrun do
  sh "jekyll --server --auto"
end
