---
layout: page
title: About
permalink: /about/
order: 4
---

再读研究生一枚。

喜欢大海，雪，和西瓜。



<!-- 添加 contact -->
<br/>

## Contact




<!-- <form method="POST" action="https://formspree.io/chen_dian_zhang@sina.com">
  <input type="email" name="email" placeholder="Your email">
  <textarea name="message" placeholder="Test Message"></textarea>
  <button type="submit">Send Test</button>
</form> -->

<contact_body style="margin-bottom: 221px;">
<form class="form" id="contactform" action="https://formspree.io/chen_dian_zhang@sina.com" method="POST">
    <fieldset class="field">
        <label class="label" for="name"><span class="label-content">Your name</span></label>
        <input class="input" type="text" name="name" placeholder="Name" required="">
    </fieldset>
    <fieldset class="field">
        <label class="label" for="_replyto"><span class="label-content">Your email</span></label>
        <input class="input" type="email" name="_replyto" placeholder="your_email@example.com" required="">
    </fieldset>
    <fieldset class="field">
        <label class="label" for="message"><span class="label-content">Your message</span></label>
        <textarea class="input" name="message" rows="7" placeholder="Message" required=""></textarea>
    </fieldset>
    <input class="hidden" type="text" name="_gotcha" style="display:none">
    <input class="hidden" type="hidden" name="_subject" value="Message via Super Tech Crew">
    <fieldset class="field">
        <input class="button submit" type="submit" value="Send">
    </fieldset>
</form>

</contact_body>

<br/>

{%- if site.email -%}
<contact_body>
Or email:
<a class="u-email" href="mailto:{{ site.email }}">{{ site.email }}</a>
</contact_body>
{%- endif -%}