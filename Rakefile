require 'markdown_helper'

RakefileDirPath = File.dirname(__FILE__)

namespace :build do

  desc 'Build markdown'
  task :markdown do
    markdown_helper = MarkdownHelper.new
    template_file_name = 'template.md'
    markdown_file_name = 'markdown.md'
    Dir.chdir(RakefileDirPath)
    contents = ['## Contents']
    current_section_name = nil
    template_file_paths = Dir.glob("**/#{template_file_name}")
    template_file_paths.each do |template_file_path|
      template_dir_path = File.dirname(template_file_path)
      section_name, topic_name = template_dir_path.split('/')
      if section_name != current_section_name
        contents.push("- #{section_name}")
        current_section_name = section_name
      end
      link_path = File.join(section_name,topic_name,'markdown.md')
      contents.push("  - [#{topic_name}](#{link_path})")
      Dir.chdir(template_dir_path) do
        markdown_helper.include(template_file_name, markdown_file_name)
        markdown_helper.run_irb(markdown_file_name, markdown_file_name)
      end
    end
    contents.push('')
    File.write('include_files/contents.md', contents.join("\n"))
    markdown_helper.include('README.template.md', 'README.md')
  end
end