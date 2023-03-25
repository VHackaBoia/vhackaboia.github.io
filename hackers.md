<div class="row">
  {% assign sorted_users = site.hackers | sort: 'role' | reverse %}
  {% for user in sorted_users %}
        <div class="col-lg-3 col-xs-4 col-sm-4 p-5">
            <div class="row">
                <a href="/hackers/{{ user.name }}" title="{{ user.name }}">
                    <img class="rounded-circle" src="/hackers/avatars/{% if user.avatar %}{{ user.avatar }}{% else %}default.png{% endif %}" alt="avatar" />
                </a>
            </div>
            <div class="my-2 text-center">
                <a href="/hackers/{{ user.name }}" title="{{ user.name }}">{{user.name}}</a>
                <br>
                {% for skill in user.skills %}
                    {% case skill %}
                        {% when "pwn" %}
                            <i class="mr-1 fa-solid fa-bug" title="pwn"></i>
                        {% when "web" %}
                            <i class="mr-1 fa-solid fa-globe" title="web"></i>
                        {% when "crypto" %}
                            <i class="mr-1 fa-solid fa-key" title="crypto"></i>
                        {% when "forensics" %}
                            <i class="mr-1 fa-solid fa-searchengin" title="forensics"></i>
                        {% when "reversing" %}
                            <i class="mr-1 fa-solid fa-code" title="reversing"></i>
                        {% when "misc" %}
                            <i class="mr-1 fa-solid fa-hot-wizard" title="misc"></i>
                        {% when "android" %}
                            <i class="mr-1 fa-solid fa-android" title="android"></i>
                        {% when "osint" %}
                            <i class="mr-1 fa-solid fa-user-secret" title="osint"></i>
                        {% when "stego" %}
                            <i class="mr-1 fa-solid fa-icons" title="stego"></i>
                        {% when "network" %}
                            <i class="mr-1 fa-solid fa-network-wired" title="network"></i>
                        {% when "hardware" %}
                            <i class="mr-1 fa-solid fa-microchip" title="hardware"></i>
                    {% endcase %}
                {% endfor %}
            </div>
       </div>
  {% endfor %}
 </div>
