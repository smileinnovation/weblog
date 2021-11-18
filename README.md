## How to write content

### Helpers

**Youtube**

Directly in the content:
 `{% youtube "https://www.youtube.com/watch?v=ho8-vK0L1_8" %}`

**Google maps**

In front-matter:
```yml
location:
  - latitude: 51.5285582
    longitude: -0.2416807
  - latitude: 52.5285582
    longitude: -2.2416807
  - title: custom marker title
    image: custom marker image
    url: custom marker url
    latitude: 51.5285582
    longitude: -0.2416807
```
Then, in your content `{% google_map %}`

**Gist**

In your content `{% gist c08ee0f2726fd0e3909d %}` or `{% gist c08ee0f2726fd0e3909d test.md %}`
