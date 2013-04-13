
# How to create a Xen image for an Elixir project

Elixir has Erlang underneath, all the way down. Thus an Elixir project can run
not only on a standard Erlang VM, but on its &ldquo;OS-less&rdquo; counterpart
called LING VM. LING VM is the core technology of Erlang on Xen -
[erlangonxen.org](http://erlangonxen.org).

This tutorial explains how to produce a Xen image for your Elixir project using
the free Erlang on Xen Build Service.

1. Add a dependency on 'lingex' to your mix.exs file:

	defp deps do
	  [ ...
	    { :lingex, github: "maximk/lingex" }
		... ]
	end

1. Run mix deps.get to update your dependencies. This adds a few custom tasks
to the mix tool (lingex.build, lingex.image, and lingex.buildi\_image)

1. Set lingex options. Add the following lines to your mix.exs file:

	def project do
	  [ ...
	    lingex_opts: [ build_host: "build.erlangonxen.org:8080",
					   username: "test",
					   password: "test" ]
		... ]
	end

1. Optionally, you may register with the build service
[here](http://build.erlangonxen.org/register) and update the credentials
accordingly. For the complete list of recognized options see the build service
documentation.

1. Run mix lingex.build\_image. This will archive all \*.beam files of your
project and submit them to the build service.

1. The build process will complete in about 30s. An image file called 'vmling'
will appear in the current directory. The file contains LING VM, your project
code and is ready to boot as a Xen guest.

A couple configuration steps are needed to automatically start the Elixir
project when the image boots. This should be covered by a separate tutorial.
