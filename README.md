# tgbot-cpp

[![Build Status](https://travis-ci.org/reo7sp/tgbot-cpp.svg?branch=master)](https://travis-ci.org/reo7sp/tgbot-cpp)
[![Join the chat at https://gitter.im/reo7sp/tgbot-cpp](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/reo7sp/tgbot-cpp?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Donate using Yandex.Money](https://i.imgur.com/8Fi2DnS.png)](https://money.yandex.ru/to/410011554060830)

C++ library for Telegram bot API.

Documentation is located [here](http://reo7sp.github.io/tgbot-cpp)

## Compilation

Firstly you need to install some dependencies such as Boost and build tools such as CMake. On Debian-based distibutives you can do it with these commands:
```sh
sudo apt-get install g++ make binutils cmake libssl-dev libboost-system-dev libboost-iostreams-dev
```

To compile the library execute this commands:
```sh
cd /path/where/you/have/cloned/the/library/repository
cmake .
make -j4
sudo make install
```

That's all. All you have to do now is just link compiled library to your project.

If you want, you can also use Docker to build and run your bot. Just set the base image of your's Dockerfile to [reo7sp/tgbot-cpp](https://hub.docker.com/r/reo7sp/tgbot-cpp/).

## Samples

Simple echo bot which sends everything it receives:
```cpp
#include <stdio.h>
#include <tgbot/tgbot.h>

int main() {
    TgBot::Bot bot("PLACE YOUR TOKEN HERE");
    bot.getEvents().onCommand("start", [&bot](TgBot::Message::Ptr message) {
        bot.getApi().sendMessage(message->chat->id, "Hi!");
    });
    bot.getEvents().onAnyMessage([&bot](TgBot::Message::Ptr message) {
        printf("User wrote %s\n", message->text.c_str());
        if (StringTools::startsWith(message->text, "/start")) {
            return;
        }
        bot.getApi().sendMessage(message->chat->id, "Your message is: " + message->text);
    });
    try {
        printf("Bot username: %s\n", bot.getApi().getMe()->username.c_str());
        TgBot::TgLongPoll longPoll(bot);
        while (true) {
            printf("Long poll started\n");
            longPoll.start();
        }
    } catch (TgBot::TgException& e) {
        printf("error: %s\n", e.what());
    }
    return 0;
}
```

All samples are located [here](samples)

## Feedback
Feel free to [create new issues on GitHub](https://github.com/reo7sp/tgbot-cpp/issues) or [contact me on Telegram](https://t.me/reo7sp)

## Licence
[The MIT License](http://opensource.org/licenses/MIT)
