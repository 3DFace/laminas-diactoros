# Changelog

All notable changes to this project will be documented in this file, in reverse chronological order by release.

## 1.0.4 - 2015-06-23

This is a security release.

A patch has been applied to `Laminas\Diactoros\Uri::filterPath()` that ensures that
paths can only begin with a single leading slash. This prevents the following
potential security issues:

- XSS vectors. If the URI path is used for links or form targets, this prevents
  cases where the first segment of the path resembles a domain name, thus
  creating scheme-relative links such as `//example.com/foo`. With the patch,
  the leading double slash is reduced to a single slash, preventing the XSS
  vector.
- Open redirects. If the URI path is used for `Location` or `Link` headers,
  without a scheme and authority, potential for open redirects exist if clients
  do not prepend the scheme and authority. Again, preventing a double slash
  corrects the vector.

If you are using `Laminas\Diactoros\Uri` for creating links, form targets, or
redirect paths, and only using the path segment, we recommend upgrading
immediately.

### Added

- [zendframework/zend-diactoros#25](https://github.com/zendframework/zend-diactoros/pull/25) adds
  documentation. Documentation is written in markdown, and can be converted to
  HTML using [bookdown](http://bookdown.io). New features now MUST include
  documentation for acceptance.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#51](https://github.com/zendframework/zend-diactoros/pull/51) fixes
  `MessageTrait::getHeaderLine()` to return an empty string instead of `null` if
  the header is undefined (which is the behavior specified in PSR-7).
- [zendframework/zend-diactoros#57](https://github.com/zendframework/zend-diactoros/pull/57) fixes the
  behavior of how the `ServerRequestFactory` marshals upload files when they are
  represented as a nested associative array.
- [zendframework/zend-diactoros#49](https://github.com/zendframework/zend-diactoros/pull/49) provides several 
  fixes that ensure that Diactoros complies with the PSR-7 specification:
  - `MessageInterface::getHeaderLine()` MUST return a string (that string CAN be
    empty). Previously, Diactoros would return `null`.
  - If no `Host` header is set, the `$preserveHost` flag MUST be ignored when
    calling `withUri()` (previously, Diactoros would not set the `Host` header
    if `$preserveHost` was `true`, but no `Host` header was present).
  - The request method MUST be a string; it CAN be empty. Previously, Diactoros
    would return `null`.
  - The request MUST return a `UriInterface` instance from `getUri()`; that
    instance CAN be empty. Previously, Diactoros would return `null`; now it
    lazy-instantiates an empty `Uri` instance on initialization.
- [ZF2015-05](https://getlaminas.org/security/advisory/ZF2015-05) was
  addressed by altering `Uri::filterPath()` to prevent emitting a path prepended
  with multiple slashes.

## 1.0.3 - 2015-06-04

### Added

- [zendframework/zend-diactoros#48](https://github.com/zendframework/zend-diactoros/pull/48) drops the
  minimum supported PHP version to 5.4, to allow an easier upgrade path for
  Symfony 2.7 users, and potential Drupal 8 usage.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- Nothing.

## 1.0.2 - 2015-06-04

### Added

- [zendframework/zend-diactoros#27](https://github.com/zendframework/zend-diactoros/pull/27) adds phonetic
  pronunciation of "Diactoros" to the README file.
- [zendframework/zend-diactoros#36](https://github.com/zendframework/zend-diactoros/pull/36) adds property
  annotations to the class-level docblock of `Laminas\Diactoros\RequestTrait` to
  ensure properties inherited from the `MessageTrait` are inherited by
  implementations.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#41](https://github.com/zendframework/zend-diactoros/pull/41) fixes the
  namespace for test files to begin with `LaminasTest` instead of `Laminas`.
- [zendframework/zend-diactoros#46](https://github.com/zendframework/zend-diactoros/pull/46) ensures that
  the cookie and query params for the `ServerRequest` implementation are
  initialized as arrays.
- [zendframework/zend-diactoros#47](https://github.com/zendframework/zend-diactoros/pull/47) modifies the
  internal logic in `HeaderSecurity::isValid()` to use a regular expression
  instead of character-by-character comparisons, improving performance.

## 1.0.1 - 2015-05-26

### Added

- [zendframework/zend-diactoros#10](https://github.com/zendframework/zend-diactoros/pull/10) adds
  `Laminas\Diactoros\RelativeStream`, which will return stream contents relative to
  a given offset (i.e., a subset of the stream).  `AbstractSerializer` was
  updated to create a `RelativeStream` when creating the body of a message,
  which will prevent duplication of the stream in-memory.
- [zendframework/zend-diactoros#21](https://github.com/zendframework/zend-diactoros/pull/21) adds a
  `.gitattributes` file that excludes directories and files not needed for
  production; this will further minify the package for production use cases.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- [zendframework/zend-diactoros#9](https://github.com/zendframework/zend-diactoros/pull/9) ensures that
  attributes are initialized to an empty array, ensuring that attempts to
  retrieve single attributes when none are defined will not produce errors.
- [zendframework/zend-diactoros#14](https://github.com/zendframework/zend-diactoros/pull/14) updates
  `Laminas\Diactoros\Request` to use a `php://temp` stream by default instead of
  `php://memory`, to ensure requests do not create an out-of-memory condition.
- [zendframework/zend-diactoros#15](https://github.com/zendframework/zend-diactoros/pull/15) updates
  `Laminas\Diactoros\Stream` to ensure that write operations trigger an exception
  if the stream is not writeable. Additionally, it adds more robust logic for
  determining if a stream is writeable.

## 1.0.0 - 2015-05-21

First stable release, and first release as `laminas-diactoros`.

### Added

- Nothing.

### Deprecated

- Nothing.

### Removed

- Nothing.

### Fixed

- Nothing.
