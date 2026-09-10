# piyushbedi.com

Static Astro site deployed from `main` to GitHub Pages.

## Local development

Use Node.js 24 and the committed npm lockfile:

```sh
npm ci
npm run dev
```

Run `npm run build` to generate `dist/`, then `npm run preview` to preview it.

## GitHub Pages setup

1. Open [repository Pages settings](https://github.com/piyushbedi/blog/settings/pages).
2. Under **Build and deployment**, select **GitHub Actions** as the source.
3. Set **Custom domain** to `piyushbedi.com` and save before changing DNS.
4. Commit and push the deployment configuration to `main`. The **Deploy to GitHub Pages** workflow builds and publishes the site; it can also be run manually from Actions after it is pushed.
5. After DNS resolves and GitHub issues the certificate, enable **Enforce HTTPS** in Pages settings. DNS propagation and HTTPS availability can take up to 24 hours.

Astro uses `https://piyushbedi.com` with the default root base (`/`), so do not add `/blog` as the base. `public/CNAME` records the intended domain in the output, but GitHub Actions deployments require the custom domain to be set in Pages settings; the file alone does not configure it.

## Hover DNS

In Hover, select **piyushbedi.com → DNS**. Keep Hover's nameservers (`ns1.hover.com` and `ns2.hover.com`). Add one record per row with the default TTL:

| Type | Hostname | Value |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | piyushbedi.github.io |

Replace conflicting parking/forwarding records for `@` and `www`. Preserve unrelated records, including email MX and TXT records. The `www` target must contain neither `https://` nor `/blog`. GitHub redirects `www.piyushbedi.com` to the apex domain.

Optional IPv6: add these four `AAAA` records at `@` alongside the A records (replace any old apex AAAA records that point elsewhere):

```text
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

## Verify deployment

- Confirm the GitHub Actions deployment succeeds.
- Confirm `https://piyushbedi.com` loads and `https://www.piyushbedi.com` redirects to it.
- Check DNS on Windows:

```powershell
Resolve-DnsName piyushbedi.com -Type A
Resolve-DnsName www.piyushbedi.com -Type CNAME
```

References: [Astro deployment guide](https://docs.astro.build/en/guides/deploy/github/), [GitHub custom domains](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site), [Hover DNS management](https://support.hover.com/support/solutions/articles/201000064728-managing-dns-records-at-hover).
