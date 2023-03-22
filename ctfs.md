---
layout: default
---

<div class="row">
{% assign sorted_ctfs = site.ctfs | sort: 'start_date' | reverse %}
    <div class="table-responsive">
        <table class="">
            <thead>
                <tr>
                    <th>Type</th>
                    <th>Name</th>
                    <th>Rank</th>
                    <th>Weight</th>
                    <th>Start Date</th>
                    <th>End Date</th>
                </tr>
            </thead>
            <tbody>
            {% for ctf in sorted_ctfs %}
                <tr>
                    <td>{{ ctf.type }}</td>
                    <td><a href="{{ ctf.url }}">{{ ctf.name }}</a></td>
                    <td>{{ ctf.rank }} / {{ctf.teams}}</td>
                    <td>{{ ctf.weight }}</td>
                    <td>{{ ctf.start_date }}</td>
                    <td>{{ ctf.end_date }}</td>
                </tr>
            {% endfor %}
            </tbody>
        </table>
    </div>
</div>