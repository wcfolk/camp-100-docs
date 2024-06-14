
This is the web root of the repository which is used to store the Camp 100 Documents. If you're looking for the actual Camp 100 website, that's over at [camp100.org.uk](https://camp100.org.uk). 

For information about this repository - go and view the [repositories wiki](https://github.com/wcfolk/camp-100-docs/wiki)

## List of Documents

{% assign staticlist = site.static_files | sort: 'url'  %}
{% for pdf in staticlist %}
{% if pdf.extname == ".pdf" %}
<a href="{{ site.url }}{{ site.baseurl }}{{ pdf.path }}">{{ pdf.path }}</a>
{% endif %}
{% endfor %}