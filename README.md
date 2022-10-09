# Summary

## Reference docs
- [ami_locator](https://cloud-images.ubuntu.com/locator/ec2/)


## AMI Build steps
- create `*.pkr.hcl` file with packer, source(s), build(s)
- make sure `~/.credrc` is configured with aws credentials
- run `packer init .` in directory with `*pkr.hcl` (pulls plugins needed)
- run `packer fmt .` (enforce formatting, best practice)
- run `packer validate .` (checks sysntax errors)
- run `packer build <my-ami.pkr.hcl>` to build the ami
- `packer build` will output the `ami-ID` when it finished the build

## AMI Provisioner
This allows you to modify the image with shell scripts or file uploads.
- `build` blocks can contain a `provisioner` to add extra configuration steps on the ami, but it requires `ssh_username` specified in the `source` that its building.  This creates temp ssh keys for it to login and execute the logic in the `provisioner`

## Variables
- two types:  input and local (standard terraform...)
- input var assignement precedence:  `defaults`<`env_vars`<`var file(s)`<`cmd-line flag`
- defaults example:
  - ```
    variable "ami_prefix" {
      type = string
      default = "this is my default name"
    }
   ```
- var file example:
  - `echo ami_prefix = "\"my_ami_prefix"\" >> example.pkvars.hcl`
  - `packer build --var-file=example.pkvars.hcl aws-ubuntu.pkr.hcl`
  - `packer` auto loads files that pattern match `*.auto.pkvars.hcl`, so `mv example.pkvars.hcl example.auto.pkvars.hcl` then run `packer build aws-ubuntu.pkr.hcl` without the `--var-file`
- cmd-line flag example:
  - `packer build --var ami_prefix=learn-packer-aws-redis-var-flag`

