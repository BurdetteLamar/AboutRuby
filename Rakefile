require 'markdown_helper'

RakefileDirPath = File.dirname(__FILE__)

namespace :build do

  desc 'Build markdown'
  task :markdown do
    markdown_helper = MarkdownHelper.new
    template_file_name = 'template.md'
    markdown_file_name = 'markdown.md'
    Dir.chdir(RakefileDirPath)
    template_file_paths = Dir.glob("**/#{template_file_name}")
    template_file_paths.each do |template_file_path|
      template_dir_path = File.dirname(template_file_path)
      Dir.chdir(template_dir_path) do
        markdown_helper.include(template_file_name, markdown_file_name)
        markdown_helper.run_irb(markdown_file_name, markdown_file_name)
      end
    end
  end
end