---
layout: post
title: Virt-manager resolution
---

Virt-manager sejauh ini adalah frontend virtualisasi favorit saya, namun sempat ada kendala yang paling sering saya hadapi, yaitu terkait resolusi. Setelah saya cari ternyata cukup mudah untuk melakukan setting resolusi di virt-manager, melalui setting xml sebagai berikut:

```html
<video>
  <model type="virtio" heads="1" primary="yes">
    <resolution x="800" y="600"/>
  </model>
  <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x0"/>
</video>
```

Cukup sesuaikan konfigurasi maka resolusi bisa di set secara manual.

saus:

[youtube.com](https://www.youtube.com/watch?v=qP87ntza5tE)
