# AMI Build steps
- create `*.pkr.hcl` file with packer, source(s), build(s)
- make sure `~/.credrc` is configured with aws credentials
- run `packer init .` in directory with `*pkr.hcl` (pulls plugins needed)
- run `packer fmt .` (enforce formatting, best practice)
- run `packer validate .` (checks sysntax errors)
- run `packer build <my-ami.pkr.hcl>` to build the ami
- `packer build` will output the `ami-ID` when it finished the build

# AMI Provisioner
This allows you to modify the image with shell scripts or file uploads.
- `build` blocks can contain a `provisioner` to add extra configuration steps on the ami, but it requires `ssh_username` specified in the `source` that its building.  This creates temp ssh keys for it to login and execute the logic in the `provisioner`.
