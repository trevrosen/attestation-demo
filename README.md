# attestation-demo

This repo provides an example of how to create and attest a container image using [GitHub Artifact Attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds).

## Container Image Tags

The GitHub Actions workflow builds and pushes the container image to GitHub Container Registry (GHCR) with two tags:

1. `latest` - Always points to the most recent successful build from the main branch
2. `sha-<commit>` - Contains the short SHA of the Git commit that triggered the build
   - Example: `ghcr.io/trevrosen/attestation-demo:sha-a1b2c3d`
   - This allows for precise tracking of which code version is running in the container

### Accessing Tagged Images

You can pull a specific version of the image using either tag:

```bash
# Pull the latest version
docker pull ghcr.io/trevrosen/attestation-demo:latest

# Pull a specific commit version
docker pull ghcr.io/trevrosen/attestation-demo:sha-<commit>
```

The short SHA tag provides immutability and traceability, making it easier to:
- Roll back to a specific version
- Track which code version is running in production
- Verify the exact source code that produced a container image

## Container Attestation

Each container image pushed to GHCR includes a attestation that provides cryptographic proof of the build process. The attestation is automatically generated and pushed by the GitHub Actions workflow and applies to both the `latest` and commit-specific tags.
