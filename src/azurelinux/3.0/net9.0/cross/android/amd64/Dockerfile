FROM mcr.microsoft.com/dotnet-buildtools/prereqs:azurelinux-3.0-net9.0-cross-amd64-local AS crossrootx64

FROM mcr.microsoft.com/dotnet-buildtools/prereqs:azurelinux-3.0-net9.0-android-local

# Copy crossrootfs from AMD64 build image, so we can build Android-targeting code for that arch
COPY --from=crossrootx64 /crossrootfs/x64 /crossrootfs/x64
