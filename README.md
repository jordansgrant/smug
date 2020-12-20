# Smug - tmux session manager.

Inspired by [tmuxinator](https://github.com/tmuxinator/tmuxinator) and [tmuxp](https://github.com/tmux-python/tmuxp).

Smug automates your tmux workflow. You can create a single configuration file, and smug will create all required windows and panes for it.

![gif](https://i.imgur.com/CfLgrz5.gif)

## Usage

`tmux <command> <project>[:window name] [-w window name]`.

## Examples

To start/stop a project and all windows, run:

`$ smug start project`

`$ smug stop project`

When you already have a running session, and you want to create only some windows from the configuration file, you can do something like this:

`$ smug start project:window1`

`$ smug start project:window1,window2`

`$ smug start project -w window1`

`$ smug start project -w window1 -w window2`

## Configuration

Configuration files are stored in the `~/.config/smug` directory in the `YAML` format, e.g `~/.config/smug/your_project.yml`.

Example:

```yaml
session: blog

root: ~/Developer/blog

before_start:
  - docker-compose -f my-microservices/docker-compose.yml up -d # my-microservices/docker-compose.yml is a relative to `root`

stop:
  - docker stop $(docker ps -q)

windows:
  - name: code
    root: blog # a relative path to root
    manual: true # you can start this window only manually, using the -w arg
    commands:
      - docker-compose start
    panes:
      - type: horizontal
        root: .
        commands:
          - docker-compose exec php /bin/sh
          - clear

  - name: infrastructure
    root: ~/Developer/blog/my-microservices
    panes:
      - type: horizontal
        root: .
        commands:
          - docker-compose up -d
          - docker-compose exec php /bin/sh
          - clear
```
