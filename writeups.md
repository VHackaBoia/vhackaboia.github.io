---
layout: default
---

<div class="row">
{% assign sorted_writeups = site.writeups | sort: 'date' %}
    <div class="table-responsive">
        <table class="">
            <thead>

            </thead>
            <tbody>
            {% for writeup in sorted_writeups %}
                <tr>
                    
                    <td>{{ writeup.category }}</td>
                    <td><a href="/ctfs/{{writeup.ctf}}">{{ writeup.ctf }}</a></td>
                    <td><a href="{{ writeup.url }}">{{ writeup.title }}</a></td>
                    <td>{{ writeup.points }}</td>
                    <td>{{ writeup.solves }}</td>
                    <td>{{ writeup.author }}</td>
                </tr>
            {% endfor %}
            </tbody>
        </table>
    </div>
</div>