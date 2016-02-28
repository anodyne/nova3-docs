# Responsive Web Design

## Custom Tags

Using Vue components, we've built custom tags that help with responsive designs and targeting specific-sized devices. Nova 3 ships with components for the following devices:

- phone
- tablet
- phone-tablet
- desktop-sm
- desktop-lg
- desktop
- tablet-desktop

These components are used just like any other HTML tag:

<pre>&lt;phone-tablet>This will only appear on phones and tablets.&lt;/phone-tablet>

&lt;desktop>This will only appear on small and large desktop screens.&lt;/desktop></pre>

This is an easy way to target different devices without needing to remember specific markup from Bootstrap about targeting different devices.
