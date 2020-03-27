require 'rake/testtask'
require 'markdown_helper'

Rake::TestTask.new(:test) do |t|
  t.libs << 'test'
  t.libs << 'lib'
  t.test_files = FileList['test/**/*_test.rb']
end

RakefileDirPath = File.dirname(__FILE__)

namespace :build do

  desc 'Build markdown'
  task :markdown do
    markdown_helper = MarkdownHelper.new(pristine: true)
    template_file_name = 'template.md'
    markdown_file_name = 'markdown.md'
    Dir.chdir(RakefileDirPath)
    template_file_paths = Dir.glob("**/#{template_file_name}")
    template_file_paths.each do |template_file_path|
      template_dir_path = File.dirname(template_file_path)
      Dir.chdir(template_dir_path) do
        markdown_helper.include(template_file_name, markdown_file_name)
      end
    end
    markdown_helper.include('README.template.md', 'README.md')
  end
end

task :default => :test
