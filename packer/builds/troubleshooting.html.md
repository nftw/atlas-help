---
title: "Troubleshooting Failing Builds"
---

# Troubleshooting Failing Builds

Packer builds can fail in Atlas for a number of reasons – improper
configuration, transient networking errors, and hardware constraints
are all possible. Below is a list of debugging options you can use.

### Verbose Packer Logging

You can [set a variable](/help/packer/builds/build-environment#environment-variables) in the UI that increases the logging verbosity
in Packer. Set the `PACKER_LOG` key to a value of `1` to accomplish this.

After setting the variable, you'll need to [rebuild](/help/packer/builds/rebuilding).

Verbose logging will be much louder than normal Packer logs and isn't
recommended for day-to-day operations. Once enabled, you'll be able to
see in further detail why things failed or what operations Packer was performing.

This can also be used locally:

    PACKER_LOG=1 packer build ...

### Hanging Builds

Some VM builds, such as VMware or Virtualbox, may hang at various stages,
most notably `Waiting for SSH...`.

Things to pay attention to when this happens:

- SSH credentials must be properly configured. AWS keypairs should
match, SSH usernames should be correct, passwords should match, etc.
- Any VM preseed configuration should have the same SSH configuration
as your template defines

A good way to debug this is to manually attempt to use the same SSH
configuration locally, running with `packer build -debug`. See
more about [debugging Packer builds](https://packer.io/docs/other/debugging.html).

### Hardware Limitations

Your build may be failing by requesting larger memory or
disk usage then is available. Read more about the [build environment](/help/packer/builds/build-environment#hardware-limitations).

### Local Debugging

Sometimes it's faster to debug failing builds locally. In this case,
you'll want to [install Packer](/help/intro/updating-tools) and any providers (like Virtualbox) necessary.

Because Atlas runs the open source version of Packer, there should be
no difference in execution between the two, other than the environment that
Packer is running in. For more on hardware constraints in the Atlas environment
read below.

Once your builds are running smoothly locally you can push it up to Atlas
for versioning and automated builds.

### Internal Errors

This is a short list of internal errors and what they mean.

- SIC-001: Your data was being ingressed from GitHub but failed
to properly unpack. This can be caused by bad permissions, using
symlinks or very large repository sizes. Using symlinks inside of the
packer directory, or the root of the repository, if the packer directory
is unspecified, will result in this internal error.
_**Note:** Most often this error occurs
when applications or builds are linked to a GitHub repository and the 
directory and/or template paths are incorrect. Double check that the paths 
specified when you linked the GitHub repository match the actual paths 
to your template file._
- SEC-001: Your data was being unpacked from a tarball uploaded to Atlas
and encountered an error. This can be caused by bad permissions, using
symlinks or very large tarball sizes.

### Community Resources

Packer is an open source project with an active community. If you're
having an issue specific to Packer, the best avenue for support is
the mailing list or IRC. All bug reports should go to GitHub.

- Website: [packer.io](https://packer.io)
- GitHub: [github.com/mitchellh/packer](https://github.com/mitchellh/packer)
- IRC: `#packer-tool` on Freenode
- Mailing list: [Google Groups](http://groups.google.com/group/packer-tool)

### Getting Support

If you believe your build is failing as a result of a bug in Atlas,
or would like other support, please [email us](mailto:support@hashicorp.com).

