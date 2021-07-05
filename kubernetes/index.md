# Welcome to Kube paradise

{% for file in site.static_files %}
{% if file.extname == ".md" %}
[{{ file.basename }}](({/kubernetes/{file.path}}).html)
{% endif %}
{% endfor %}    