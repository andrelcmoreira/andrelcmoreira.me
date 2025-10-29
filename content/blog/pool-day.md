+++
title = "Managing threads in C/C++ applications with pool-day"
date = "2025-11-18"
+++

### Overview

TODO

### Build and Installation

The library relies on `cmake` tool to be built:

```bash
$ cmake -S . -B build
$ cmake --build build
$ sudo cmake --install build
```

Additional flags can be supplied as parameter to the build command, according to
the table below:

<table style="width:55%; border-collapse: collapse;">
  <thead>
    <tr style="background-color:#e2e2e2;">
      <th style="padding: 8px; border: 2px solid #ddd; text-align: center;">Flag</th>
      <th style="padding: 8px; border: 2px solid #ddd; text-align: center;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 3px solid #ddd; text-align: center">BUILD_SAMPLES</td>
      <td style="padding: 8px; border: 3px solid #ddd;">Build the library's samples</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 3px solid #ddd; text-align: center">ENABLE_LOGGING</td>
      <td colspan="2" style="padding: 8px; border: 3px solid #ddd;">Enable the library's logging feature</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 3px solid #ddd; text-align: center">BUILD_PYTHON_BINDINGS</td>
      <td colspan="2" style="padding: 8px; border: 3px solid #ddd;">Build the library's python bindings</td>
    </tr>
  </tbody>
</table>

### Usage

TODO

### Examples

TODO

### Conclusion

TODO
