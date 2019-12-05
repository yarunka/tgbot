# -*- coding: utf-8 -*-
import telebot
import pyowm
from pyowm.exceptions import api_response_error
from telebot import types

TOKEN = "670851332:AAGUG5iCVsN4YuVOSo9ExqP6_JVrBRjFCrw"
my_own_key = "db9600d52b92a3f7e1d443fd2728a64e"

owm = pyowm.OWM(my_own_key)
bot = telebot.TeleBot(TOKEN)


@bot.message_handler(commands=['start'])
def start(message):
    sent = bot.send_message(message.chat.id, 'В якому місті погоду будем дивитись?')
    bot.register_next_step_handler(sent, check)

def check(message):
    try:
        if owm.weather_at_place(message.text):
            observation = owm.weather_at_place(message.text)
            w = observation.get_weather()
            temp = w.get_temperature('celsius')["temp"]
            temp_max = w.get_temperature('celsius')["temp_max"]
            temp_min = w.get_temperature('celsius')["temp_min"]

            answer = 'Середня тeмпература в місті ' + message.text + " на даний момент:\n {temperature} градусів по Цельсію\n\n".format(
                temperature=temp)
            answer += 'Максимальна тeмпература на яку можна очікувати найближчим часом:\n {temperature} градусів по Цельсію\n\n'.format(
                temperature=temp_max)
            answer += 'Мінімальна тeмпература  на яку варто надіятись:\n {temperature} градусів по Цельсію\n\n'.format(
                temperature=temp_min)

            bot.send_message(message.chat.id, answer)

            markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
            markup.add('Так', 'Ні')
            msg = bot.reply_to(message, 'Попробуєш інше місто?', reply_markup=markup)
            bot.register_next_step_handler(msg, what_next)

    except api_response_error.NotFoundError:
        markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
        markup.add('Так', 'Ні')
        msg = bot.reply_to(message, 'Немає такого міста. Попробуєш ще раз?', reply_markup=markup)
        bot.register_next_step_handler(msg, what_next)


def what_next(answer):
    if answer.text == "Так":
        sent = bot.send_message(answer.chat.id, 'В якому місті погода тебе цікавить?')
        bot.register_next_step_handler(sent, check)

    if answer.text == "Ні":
        bot.send_message(answer.chat.id, "Ну і ладно. Напиши Яринці шо да як")

        markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
        markup.add("Повторити")
        msg = bot.send_message(answer.chat.id, "Ну можна повторити", reply_markup=markup)
        bot.register_next_step_handler(msg, againe)

def againe (botton):
    if botton.text == "Повторити":
        sent = bot.send_message(botton.chat.id, 'В якому місті погоду будем дивитись?')
        bot.register_next_step_handler(sent, check)
