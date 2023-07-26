# Build Marvel MDIO driver mvmdio_uio.ko for Omnia10 with Debian10

## Manual build
You can build the driver module by unpacking `net_driver_build_headers_and_module.tar.gz` and following the instructions of the containing `README.md` which - in the best case - is as simple as
```
tar -zxf net_driver_build_headers_and_module.tar.gz
cd net_driver
make
```

Then add the module `mvmdio_uio.ko` manually to your install package. That's what I would do!

`net_driver_build_headers_and_module.tar.gz` also contains the Debian linux headers and instructions how to rebuild them shall need arise.
