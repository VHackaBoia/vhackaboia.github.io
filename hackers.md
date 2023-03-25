<div class="row">
  {% assign sorted_users = site.hackers | sort: 'role' | reverse %}
  {% for user in sorted_users %}
        <div class="col-lg-3 col-xs-4 col-sm-4 p-5">
            <div class="row">
                <a href="/hackers/{{ user.name }}" title="{{ user.name }}">
                    <img class="rounded-circle" src="/hackers/avatars/{% if user.avatar %}{{ user.avatar }}{% else %}default.png{% endif %}" alt="avatar" />
                </a>
            </div>
            <div class="row">
                <div class="col-12 my-2 text-center">
                    <a href="/hackers/{{ user.name }}" title="{{ user.name }}">{{user.name}}</a>
                </div>
            </div>
            <div class="row">
                <div class="col-12 text-center">
                {% for skill in user.skills %}
                    {% for item in site.skills %}
                        {% if item.name == skill %}
                            <i class="mr-1 {{item.font-awesome}}" title="{{item.tag}}"></i>
                        {% endif %}
                    {% endfor %}
                {% endfor %}
                </div>
            </div>
       </div>
  {% endfor %}
 </div>
