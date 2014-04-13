require "date"
require "yaml"

namespace :locations do
  desc "Add a location"
  task :add do
    content = ["---"]

    print "   Name: "
    content << "name: " + (name = STDIN.gets.chomp)

    print "Address: "
    content << "address: #{STDIN.gets.chomp}"

    print "   Link: "
    content << "link: #{STDIN.gets.chomp}"

    content << "visited: false"
    content << "---"

    date     = Date.today.strftime("%Y-%m-%d")
    filename = name.downcase.gsub(" ", "-").gsub(/[^A-Za-z\-]/, "")
    filepath = "_posts/#{date}-#{filename}.md"

    File.open(filepath, "w+") { |f| f << content.join("\n") }
  end

  desc "Choose a random location"
  task :pick do
    locations = Dir[File.dirname(__FILE__) + "/_posts/*"].map { |path| YAML.load_file(path).merge("path" => path) }
    locations.delete_if { |location| location["visited"] == true }

    if locations.empty?
      puts "No locations left"
      exit 0
    end

    location = locations.sample
    puts location["name"]
    puts location["address"]

    File.open(location.delete("path"), "w+") do |f|
      f << YAML.dump(location.merge("visited" => true))
      f << "---"
    end

    config = YAML.load_file("_config.yml")

    File.open(File.dirname(__FILE__) + "/_config.yml", "w+") do |f|
      f << YAML.dump(config.merge("current" => location["name"]))
    end
  end
end
