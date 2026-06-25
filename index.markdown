---
layout: home
---

<div class="title">
  <h1>{{ site.title }}</h1>
  <p class="description">{{ site.description }}</p>
</div>

<div class="group games">
  <h2>Trading Card Game</h2>
  <a href="{{ site.url | append: '/digimon' }}" aria-label="ir para digimon">
    <img src="{{ site.url | append: '/logos/digimon.svg' }}" width="450" alt="digimon logo" />
  </a>

  <a href="{{ site.url | append: '/gundam'}}" aria-label="ir para digimon">
    <img src="{{ site.url | append: '/logos/gundam.png' }}" width="250" alt="gundam logo"/>
  </a>
</div>

{% assign events = site.posts | where: "layout", "event" %}
{% include next_events.html %}




{% assign stores = site.posts | where: "layout", "store" %}
{% include list_stores.html %}
