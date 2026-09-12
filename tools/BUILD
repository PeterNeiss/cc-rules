tools = {
    "please_cc": {
        "version": "0.1.2",
        "hashes": {
            "darwin_amd64": "89214821ff2f9862debac7ccf58a87c463c26c948bf738b2432ce48630778f0b",
            "darwin_arm64": "b5c7448c595d11fb4af618789368d1705e9c827d44b2af24ad97b23629107893",
            "freebsd_amd64": "d42fa6f139e5faec8baa6e3f0b011d34d78fe3ab5ff92da565d4efbaa78cebdc",
            "linux_amd64": "c648ac2b393813ab93e1a69032c634f0acc933a411b9a19cf95d7787ef310418",
            "linux_arm64": "b7ee0bfa5731f17c7083d3ab978f468968ac70079fe40d688d824ec840704c98",
            "windows_amd64": "9688d2b68d5bae8fda1766a44930349bf272dee4b4ac7e875494ebadbdd4ebd1",
        },
    },
}

# Upstream publishes no windows_amd64 build of these, and without one a native Windows plz
# cannot build a C or C++ target at all - the tool it needs has nothing to download. This fork
# publishes that one asset and nothing else, so only that architecture is redirected here. When
# upstream publishes its own, this map goes away.
owners = {"windows_amd64": "PeterNeiss"}

for name, metadata in tools.items():
    version = metadata["version"]
    for a, h in metadata["hashes"].items():
        # HOSTOS/HOSTARCH, not OS/ARCH: please_cc is a build tool, so it runs on the machine
        # doing the building whatever is being built for. Keyed on the target instead, a
        # cross-build to a platform with no published release defines no target called
        # please_cc at all, and every rule that uses it fails with a missing target.
        native = f"{CONFIG.HOSTOS}_{CONFIG.HOSTARCH}" == a
        owner = owners.get(a, "please-build")
        remote_file(
            name = name if native else f"{name}_{a}",
            binary = True,
            hashes = [h],
            # The downloaded name carries the version and platform, which on Windows leaves it
            # with no extension in PATHEXT - and Go's exec will not run such a file even given
            # its full path. Every other platform is happy with the name it arrives under.
            out = f"{name}.exe" if a.startswith("windows") else None,
            url = f"https://github.com/{owner}/cc-rules/releases/download/{name}-v{version}/{name}-{version}-{a}",
            visibility = ["PUBLIC"] if native else None,
        )
