# Main Development Docker

This is a docker compose file that I use as part of my local development process. Since I don't want to come up with random ports for each project of mine, I can use the power of Traefik as a reverse proxy and the power of Docker Networking

All you need to do are these things
- cd to the projects subfolder and then run `docker compose up` which will start the Traefik gateway. It will then listen on port 80 for all your local projects that you want to access through browser or other web/api clients (like Postman).
- in order for this to work for your projects, you need to do few simple things:
    -   Add one label like this in your web service (nginx for example or laravel.test for Laravel Sail or any other PHP or Python server that you want to host on port 80):
``
      labels:
          - "traefik.enable=true"
          - "traefik.http.routers.example_project.rule=Host(`example_project.localhost`)"
          - "traefik.http.routers.example_project.entrypoints=web"
          - "traefik.docker.network=projects_development-net"
``
    - Add the existing network from this project:
``
networks
    projects_development-net:
        external: true
``
and add it to your specific web service under networks (could be nginx service or laravel.test if you work with sail or any other PHP or Python server that you want to host on port 80):
``
    networks
        - projects_development-net
``

    - Then you can add `example_project.localhost` to your hosts file, start your specific project's docker compose and just visit: `http://example_project.localhost`.
    - And now you won't have to remember any port for each of your gazillion projects on your machine. And if you're smart about it, you can also use it on your production or development servers ;)
    
Cheers, Goran
