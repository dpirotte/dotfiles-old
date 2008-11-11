require 'rake'

desc "install dotfiles to home directory"
task :install do
  replace_all = false

	Dir['*'].each do |file|
		next if %w[Rakefile README LICENSE].include? file

		if File.exists?(dotfile(file))
			# don't re-link if we're already linked to this directory
			if File.symlink?(dotfile(file)) && File.readlink(dotfile(file)) == File.expand_path(file)
				puts "skipping ~/.#{file} (already linked)"
				next
			end

			if replace_all
				backup_file(file)
				link_file(file)
			else
				print "overwrite ~/.#{file}? [yNaq] "
				case $stdin.gets.chomp
				when 'a'
					replace_all = true
					backup_file(file)
					link_file(file)
				when 'y'
					backup_file(file)
					link_file(file)
				when 'q'
					exit
				else
					puts "skipping ~/.#{file}"
				end 
			end

		else
			link_file(file)
		end
  end
end

def backup_file(file)
	File.rename(dotfile(file), "#{dotfile(file)}.bak")
end

def link_file(file)
	File.symlink(File.expand_path(file), dotfile(file))
end

def dotfile(file)
  File.join(ENV['HOME'], ".#{file}")
end
