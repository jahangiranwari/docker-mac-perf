# Docker for Mac Performance Test

A quick performance test to compare the performace of volume mounts in Mac OS.

## Enable Mac OS Native NFS
```sh
$ curl -sL https://gist.githubusercontent.com/seanhandley/7dad300420e5f8f02e7243b7651c6657/raw/fdd77fe66cf9ce893fa0175d735cbede2bb065e4/setup_native_nfs_docker_osx.sh -o /tmp/setup_native_nfs_docker_osx.sh
$ sed -i.bak '53s#Users#System/Volumes/Data/Users#' /tmp/setup_native_nfs_docker_osx.sh
$ sh /tmp/setup_native_nfs_docker_osx.sh
```

## Host mount
```sh
$ docker-compose run --rm cli-host-mount sh
/home # dd if=/dev/zero of=./output bs=8k count=10k; rm ./output
10240+0 records in
10240+0 records out
83886080 bytes (80.0MB) copied, 3.084522 seconds, 25.9MB/s
```

## Host mount (NFS)
```sh
$ docker-compose run --rm cli-nfs-mount sh
/home # dd if=/dev/zero of=./output bs=8k count=10k; rm ./output
10240+0 records in
10240+0 records out
83886080 bytes (80.0MB) copied, 1.426896 seconds, 56.1MB/s
```

**Result:** When using Native NFS we can achieve over 100% performance boost.
