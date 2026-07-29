---
layout: default
---

## Getting Started

{{ site.description }}

Add the repository:

```console
$ helm repo add {{ site.repo_name }} {{ site.url }}
$ helm repo update
```

Current release **2026.7.29** — 199 charts against `base-chart` 0.4.2. The earlier
`0.2.1` / `0.2.2` / `0.2.3` sets are still listed and still resolvable, but they
predate a fix to the volume schema and **will not render**; use `2026.7.29` or later.

A chart's `appVersion` is the date the linuxserver.io image last changed, taken from
its upstream changelog. It is not the application's own version number — upstream does
not publish one, and every image is deployed from a rolling tag.

## Charts

{% for helm_chart in site.data.index.entries %}
{% assign title = helm_chart[0] | capitalize %}
{% assign all_charts = helm_chart[1] | sort: 'created' | reverse %}
{% assign latest_chart = all_charts[0] %}

<h3>
  {% if latest_chart.icon %}
  <img src="{{ latest_chart.icon }}" style="height:1.2em;vertical-align: text-top;" alt="" />
  {% endif %}
  {{ title }}
</h3>

{{ latest_chart.description }}

```console
$ helm install myrelease {{ site.repo_name }}/{{ latest_chart.name }} --version {{ latest_chart.version }}
```

| Chart Version | App Version | Published |
| ------------- | ----------- | --------- |
{% for chart in all_charts -%}
{% unless chart.version contains "-" -%}
| [{{ chart.name }}-{{ chart.version }}]({{ chart.urls[0] }}) | {{ chart.appVersion }} | {{ chart.created | date: "%Y-%m-%d" }} |
{% endunless -%}
{% endfor %}
{% endfor %}
