# GraalVM Docker Images

![GraalVM](https://img.shields.io/badge/GraalVM-Ready-orange)

| Repository | Link |
|-------------|------|
| GitHub | https://github.com/tackleza/docker-graalvm |
| Docker Hub | https://hub.docker.com/r/tackleza/graalvm |

Docker images for Oracle GraalVM on AlmaLinux 9.

**Docker Hub:** https://hub.docker.com/r/tackleza/graalvm

## Available Tags

| Tag | Description |
|-----|-------------|
| `25-almalinux` | Oracle GraalVM JDK 25 LTS; minimum 25.0.5.0.3 |
| `24-almalinux` | Retired source; excluded from supported builds |
| `21-almalinux` | Retired source; excluded from supported builds |
| `17-almalinux` | Retired source; excluded from supported builds |
| `latest` | Alias for `25-almalinux` |

## Security updates (2026-10-28)

Only `25-almalinux` and `latest` are rebuilt and published by CI. Historical
17/21 downloads were frozen on old security releases, and JDK 24 is non-LTS.
Their build scripts now stop with migration guidance. A fresh Oracle GraalVM
21 download may require different licensing terms; this project does not
silently migrate it or assume an entitlement. Test JDK 25 compatibility.

`sh build-supported.sh` pulls and rebuilds supported images without cache,
checks the resulting runtime, and does not push. Add `--push` to publish only
after every requested image passes. CI runs daily and on push/manual dispatch.
Builds fail closed unless Java reports **25.0.5.0.3 or newer in 25.x**, the
corrected October 26 security patch. A stale download endpoint or published
parent therefore cannot produce a newly tagged unpatched image.

Verification on 2026-10-28: the Oracle `25/latest` download and the published
`tackleza/graalvm:25-almalinux` parent both reported **25.0.4**, so the new
minimum-version check rejected them. No patched GraalVM image was produced.
The Oracle archive URL for 25.0.5.0.3 returned HTTP 404. Obtain an available
corrected Oracle artifact (the base Dockerfile accepts `GRAALVM_TAR_URL`),
verify/build the base, publish it, then rebuild the extended images.

This source update does not patch published images, running services, or native
executables built with older Native Image toolchains. Rebuild affected native
executables with the patched toolchain and redeploy consumers separately.

Sources: [Oracle GraalVM 25 release notes](https://docs.oracle.com/en/graalvm/jdk/25/docs/release-notes/),
[release calendar](https://www.graalvm.org/release-calendar/),
[Oracle GraalVM licensing](https://www.oracle.com/downloads/licenses/graal-free-license.html).

## Usage

### Check Java Version

```bash
docker run -it tackleza/graalvm:25-almalinux
```

```
Expected: Oracle GraalVM JDK 25.0.5.0.3 or newer in 25.x.
Verify the actual output for each image build.
```

### Interactive Shell

```bash
docker run -it tackleza/graalvm:25-almalinux bash
```

### Run a Command

```bash
docker run -it tackleza/graalvm:25-almalinux bash -c "java -version && echo 'Done'"
```

## Timezone

All supported tags include timezone data (`tzdata`) and default to **UTC**.
Set the `TZ` environment variable to a valid IANA timezone name to select another
zone. For Thailand, use `Asia/Bangkok` (UTC+07:00).

### Docker example: Bangkok / Thailand

```bash
docker run --rm -e TZ=Asia/Bangkok tackleza/graalvm:25-almalinux date
```

### Docker Compose example

```yaml
services:
  app:
    image: tackleza/graalvm:25-almalinux
    environment:
      TZ: Asia/Bangkok
    command: ["date"]
```

`TZ` sets the default timezone for OS tools and Java. An explicit Java option such
as `-Duser.timezone=UTC` overrides the JVM default; application-specific timezone
settings may also override it. Recreate the container after changing its `TZ`
environment variable.

## Example Tags

### GraalVM 25 LTS

```bash
docker run -it tackleza/graalvm:25-almalinux
```

## Base Image

These images are built on top of `almalinux:9` and include Oracle GraalVM binaries. Use them as base images for your Java/GraalVM projects.

## License

The downloaded Oracle GraalVM distribution is subject to its [Oracle GraalVM license terms](https://www.oracle.com/downloads/licenses/graal-free-license.html); it is not Community Edition.
