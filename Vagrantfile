# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_ACCESS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET_KEY']
    aws.keypair_name = "aws_key_pair"
    aws.region = "us-west-2"
    aws.instance_type = "t1.micro"
    aws.tags = {
      'Name' => 'Ubuntu',
    }

    # http://cloud-images.ubuntu.com/locator/ec2/
    aws.ami = "ami-b40c9084"

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "~/.ssh/aws_key_pair.pem"
  end

  # Enabling the Berkshelf plugin. To enable this globally, add this configuration
  # option to your ~/.vagrant.d/Vagrantfile file
  config.berkshelf.enabled = true

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      :rvm => {
        default_ruby: "system",
        rubies: ["ruby-2.0.0-p247", "jruby"],
        global_gems: [
                      {'name' => 'rails'},
                     ],
        rvm_gem_options: "--rdoc --ri",
      },
    }

    chef.run_list = [
                     "recipe[vagrant_aws::default]",
                     "nginx",
                     "php",
                     # "phpmyadmin",
                     "rvm::system",
                    ]
  end
end
