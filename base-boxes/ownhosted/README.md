
# For virtualbox

* `echo 'Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
  #  config.ssh.username = "vagrant"
  #  config.ssh.password = "vagrant"
end' >> Vagrantfile`

* I also had to add the following on lxc
  `echo 'Vagrant.configure("2") do |config|
     config.ssh.private_key_path = "~/.vagrant.d/insecure_private_key"
   end' >> Vagrantfile`

* Start your vagrant project and let the provisioning run
* `vagrant ssh -c 'cd && wget --no-check-certificate https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub -O .ssh/authorized_keys'`


* `vagrant halt`
* Do `vagrant package --output 'mybox_0.1.0.box'`
* Upload it to your webserver
* Create a whatever.json with the following content
```
 {
    "name": "project/mybox",
    "description": "This box does something",
    "versions": [{
        "version": "0.1.0",
        "providers": [{
                "name": "virtualbox",
                "url": "https://boxes.example.com/path/to/mybox_0.1.0.box",
                "checksum_type": "sha1",
                "checksum": "b58676b903c78aa275da3117d5cb31d4217aaa23"
        }]
    },
    {
        "version": "0.2.0",
        "providers": [{
                "name": "virtualbox",
                "url": "https://boxes.example.com/path/to/mybox_0.2.0.box",
                "checksum_type": "sha1",
                "checksum": "72a0a24d285728aed455f366321a29f50096c421"
        }, {
                "name": "lxc",
                "url": "https://boxes.example.com/path/to/mybox_lxc_0.2.0.box",
                "checksum_type": "sha1",
                "checksum": "14fdd7b4808a6768a70b80a55af05a655609b04e"
        }
        ]           
    }
]
}
```
* (create the checksum whith the sha1sum command line tool)
* Set the URL in your Vagrantfile to https://boxes.example.com/whatever.json and the name to "project/mybox". For drifter, you can do that in `virtualization/parameters.yml` like this:
```
lxc_box_name: "project/mybox"
lxc_box_url: "https://boxes.example.com/whatever.json"
vbox_box_name: "project/mybox"
vbox_box_url: "https://boxes.example.com/whatever.json"
```

* done

# For lxc

* do the same as above, just with `name: lxc`




