# Architecture

## Overview

This repository provides a statically compiled wkhtmltopdf binary specifically built for **ARM64 (aarch64) architecture** systems. It serves as a Composer package to simplify distribution and usage of wkhtmltopdf in PHP projects running on ARM64 platforms.

## Architecture

### Binary Distribution Model

The repository follows a simple but effective architecture:

```
wkhtmltopdf-arm64/
├── bin/
│   └── wkhtmltopdf-arm64          # Pre-compiled ARM64 binary
├── WKHTMLToPDF.php                # PHP wrapper class
├── composer.json                  # Package metadata
└── README.md                      # Usage documentation
```

### Components

1. **Pre-compiled Binary** (`bin/wkhtmltopdf-arm64`)
   - ELF 64-bit LSB pie executable for ARM aarch64
   - Dynamically linked against system libraries
   - Built for GNU/Linux 3.7.0+
   - Size: ~515KB (stripped)
   - Based on wkhtmltopdf 0.12.6

2. **PHP Wrapper Class** (`WKHTMLToPDF.php`)
   - Provides a simple constant `PATH` to locate the binary
   - Namespace: `houseoftech\WKHTMLToPDF`
   - Facilitates integration with PHP applications

3. **Composer Integration**
   - PSR-4 autoloading support
   - Binary automatically available via `vendor/bin/wkhtmltopdf-arm64`
   - No runtime dependencies required

## How This Differs From Other Distributions

### 1. **ARM64 Architecture Focus**

**This Distribution:**
- Specifically compiled for ARM64/aarch64 processors
- Optimized for ARM-based servers, Raspberry Pi 4+, AWS Graviton, Apple Silicon (via emulation), etc.

**Other Distributions:**
- Most wkhtmltopdf packages target x86_64 (amd64) architecture
- Examples: `h4cc/wkhtmltopdf-amd64`, `h4cc/wkhtmltopdf-i386`
- Will not work on ARM64 systems without emulation

### 2. **Static Compilation Approach**

**This Distribution:**
- Uses dynamically linked binary (relies on system libraries like ld-linux-aarch64.so.1)
- Smaller binary size (~515KB)
- Requires compatible system libraries on target system

**Other Approaches:**
- Some distributions use fully static binaries (larger size, no dependencies)
- Some require separate installation of Qt/WebKit libraries
- Some use Docker containers for isolation

### 3. **Composer-Based Distribution**

**This Distribution:**
- Delivered via Composer/Packagist
- Binary included directly in package
- Version controlled via Git tags
- Simple installation: `composer require houseoftech/wkhtmltopdf-arm64`

**Other Approaches:**
- System package managers (apt, yum): `wkhtmltopdf` package
- Manual download from wkhtmltopdf.org
- Docker images with wkhtmltopdf pre-installed
- Building from source

### 4. **PHP-First Integration**

**This Distribution:**
- Designed specifically for PHP projects
- Provides PHP class for easy path resolution
- Integrates seamlessly with PHP-based PDF generation libraries
- PSR-4 autoloading compatible

**Other Approaches:**
- Generic binaries without language-specific wrappers
- Require manual path configuration
- May need wrapper libraries installed separately

### 5. **Use Case Optimization**

**Best For:**
- PHP applications deployed on ARM64 servers
- Cloud environments using ARM processors (AWS Graviton, Oracle Cloud ARM)
- Raspberry Pi-based web servers
- Apple Silicon development environments (with Linux VM)
- Cost-effective cloud deployments (ARM instances are typically cheaper)

**Not Suitable For:**
- x86/x86_64 systems (use `h4cc/wkhtmltopdf-amd64` instead)
- Windows servers
- Systems without required shared libraries

## Technical Details

### Binary Characteristics

- **Format:** ELF 64-bit LSB pie executable
- **Architecture:** ARM aarch64
- **Linking:** Dynamically linked
- **Interpreter:** `/lib/ld-linux-aarch64.so.1`
- **Target OS:** GNU/Linux 3.7.0+
- **Strip Status:** Stripped (debug symbols removed)

### Dependencies

The binary requires the following system components:
- ARM64/aarch64 Linux kernel (3.7.0+)
- Dynamic linker/loader: `ld-linux-aarch64.so.1`
- Standard system libraries (glibc, libstdc++, etc.)
- X11 libraries (for rendering)
- Qt/WebKit libraries

### Version Alignment

The package version (Git tag) corresponds to the wkhtmltopdf version:
- Package version: 0.12.6
- wkhtmltopdf version: 0.12.6

## Comparison Matrix

| Feature | This Package | h4cc/wkhtmltopdf-amd64 | System Package | Docker | From Source |
|---------|--------------|------------------------|----------------|--------|-------------|
| **Architecture** | ARM64 only | x86_64 only | Various | Various | Any |
| **Installation** | Composer | Composer | apt/yum | Docker pull | Build tools |
| **Size** | ~515KB | ~40MB | Varies | ~200MB+ | N/A |
| **Dependencies** | System libs | Static | System managed | Self-contained | Many |
| **PHP Integration** | Native | Native | Manual | Manual | Manual |
| **Updates** | Git tags | Git tags | Package manager | Image tags | Manual |
| **Isolation** | None | None | None | Full | None |

## Migration Guide

### From x86_64 to ARM64

If migrating from `h4cc/wkhtmltopdf-amd64`:

```bash
# Remove x86_64 version
composer remove h4cc/wkhtmltopdf-amd64

# Add ARM64 version
composer require houseoftech/wkhtmltopdf-arm64 "0.12.6"
```

Update your code:
```php
// Old
$path = \h4cc\WKHTMLToPDF\WKHTMLToPDF::PATH;

// New
$path = \houseoftech\WKHTMLToPDF\WKHTMLToPDF::PATH;
```

### System Requirements Checklist

Before using this package, verify:
- [ ] Running on ARM64/aarch64 architecture
- [ ] Linux kernel 3.7.0 or newer
- [ ] Required system libraries installed
- [ ] X11 libraries available (for headless, use xvfb)

## Future Considerations

Potential improvements for this architecture:
1. **Fully Static Binary:** Remove system library dependencies
2. **Multi-Architecture Support:** Provide fat binary or architecture detection
3. **Version Automation:** Automated builds for new wkhtmltopdf releases
4. **Health Check Script:** Verify system compatibility before usage
5. **Extended Documentation:** More examples and troubleshooting guides

## References

- [wkhtmltopdf Official Site](http://wkhtmltopdf.org/)
- [ARM Architecture](https://en.wikipedia.org/wiki/AArch64)
- [Composer Documentation](https://getcomposer.org/doc/)
- [PSR-4 Autoloading](https://www.php-fig.org/psr/psr-4/)
