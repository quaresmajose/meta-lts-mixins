meta-lts-mixins - wrynose/linux-firmware
========================================

"Mixin" layer for adding latest Linux firmware into the Yocto Project LTS.

At the time Wrynose was released in May 2026 it included Linux firmware 20260410,
and officially Wrynose supports only that. This thin special-purpose mixin layer
is meant to provide a current Linux firmware for Wrynose by backporting the
linux-firmware recipe from the master branch of openembedded-core.

In order to make this layer Yocto Project Compatible, the layer should not provide
new versions of packages by default. Because of this, the recipe provided in this
layer will not be used unless that is explicitly stated. To use the provided
linux-firmware recipe, the following can be done:

```
# please use 'DEFAULT_PREFERENCE:your_override = ""' to keep Yocto Project Compatible
echo 'DEFAULT_PREFERENCE = ""' >> recipes-kernel/linux-firmware/linux-firmware_%.bbappend
```

Dependencies
------------

This layer depends on:

- URI: git://github.com/openembedded/openembedded-core.git
  layers: meta
    branch: wrynose

Backporting
-----------

There are breaking changes in master in the license syntax.
Because of this we carry a local backport branch
that follows oe-core/master without any local changes.

The patches can be backported from openembedded-core in two steps:

First the patch is backported on the backport branch
```
 git checkout wrynose/linux-firmware-master
 git -C ../openembedded-core format-patch --stdout -1 \
   origin/master meta/recipes-kernel/linux-firmware | \
  git am -s -p4 --directory=recipes-kernel/linux-firmware
```

Second we merge the backport branch and fix the license syntax
```
 git checkout wrynose/linux-firmware
 git merge -s -X ours wrynose/linux-firmware-master
 sed -i -e 's/AND /\& /g' -e 's/LicenseRef-//g' */*/*.bb && \
  git commit -s -m "linux-firmware: fix license syntax" -a
```

Contributing
------------

  The yocto-patches mailinglist (yocto-patches@lists.yoctoproject.org) is used
  for questions, comments and patch review. It is subscriber only, so please
  register before posting.

  Send pull requests to yocto-patches@lists.yoctoproject.org with
  '[meta-lts-mixins][wrynose/linux-firmware]' in the subject.

  When sending single patches, please use something like:

```
  git send-email -M origin/wrynose/linux-firmware \
   --to=yocto-patches@lists.yoctoproject.org \
   --subject-prefix='meta-lts-mixins][wrynose/linux-firmware][PATCH'
```

Maintenance
-----------

Layer maintainers:
* Jose Quaresma <jose.quaresma@oss.qualcomm.com>
* Viswanath Kraleti <viswanath.kraleti@oss.qualcomm.com>
