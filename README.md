# Решение тестовых заданий
## Задание 1
Потребовалось ~ 20 минут 
### Ошибка 1 (Если это именно ошибка, а не особенность форматирования в PDF)
```
dialog "Я что, похож на того, кто выдаст такие важные сведения первому
встречному? "
```
При переносе строки в данном формате будет ошибка, поэтому стоит оформить ее иначе:
```
dialog "Я что, похож на того, кто выдаст такие важные сведения первому встречному? "
```
### Ошибка 2
В конце второго блока NPC, в строке вместо dialog написано dia и в конце не поставлены кавычки, что приведет к ошибке:
```
dia "Что?!
```
Вместо этого должно быть:
```
dialog "Что?! "
```

### Ошибка 3
Неправильное использование `case` в `choose menu`
```
choose menu "Спросить о стоявшем перед ним "
case 3
break
endchoose
```
Так как меню имеет один вариант, он должен быть case 1, а не case 3. В итоге правильное оформление кода будет выглядеть так:
```
choose menu "Спросить о стоявшем перед ним "
case 1
break
endchoose
```

### Ошибка 4
У NPC Мишка#3 нет указанных координат:
```
npc "ein_dun01" "Мишка#3 "
```
Исправленная версия с примерными координатами:
```
npc "ein_dun01" "Мишка#3 " 4_NASARIAN 188 81 7 0 0
```

### Ошибка 5
В блоке перемещения, строка с командой moveto выглядит неполной:
```
moveto "ein_d02_i" 168
```
Для перемещения необходимо иметь координату Y в том числе, например:
```
moveto "ein_d02_i" 168 150 
```

### Ошибка 6 
Во всем коде иногда присутствуют лишние отступы, которые могут в теории привести к неправильному исполнению, для примера:
```
if (q11556 == 1)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Вставайте в очередь! "
        wait
        choose menu "Спросить, почему здесь очередь " "Уйти "
        case 1
        break
        endchoose
        dialog "[Плюшевый мишка в очереди] "
        dialog "Что? "
    dialog "Я что, похож на того, кто выдаст такие важные сведения первому встречному? "
```
Их нужно оформить правильно, т.е.:
```
if (q11556 == 1)
    dialog "[Плюшевый мишка в очереди] "
    dialog "Вставайте в очередь! "
    wait
    choose menu "Спросить, почему здесь очередь " "Уйти "
    case 1
    break
    endchoose
    dialog "[Плюшевый мишка в очереди] "
    dialog "Что? "
    dialog "Я что, похож на того, кто выдаст такие важные сведения первому встречному? "
```
### Ошибка 7
```
dialog "[Житель] "
dialog "Однажды все здесь выстроились в очередь... И я со всеми. Чем я хуже? "
dialog "Спрашиваете, что впереди? "
dialog "Неизвестно. Я тоже хочу это узнать, потому и стою здесь. "
```
Возможно тут не хватает wait для лучшей структуры диалога:
```
dialog "[Житель] "
dialog "Однажды все здесь выстроились в очередь... И я со всеми. Чем я хуже? "
wait
dialog "[Житель] "
dialog "Спрашиваете, что впереди? "
dialog "Неизвестно. Я тоже хочу это узнать, потому и стою здесь. "
```

### Полностью исправленный код 
```
npc "ein_dun01" "Плюшевый мишка#ITB10 " 4_NASARIAN 198 79 3 0 0
OnInit:
    AddQuestIDCondition 11556
    SetQuestConditionBegin 11556 1 0
    SetQuestConditionQuest 11556 1
    SetQuestConditionEnd
return

OnClick:
    var q11556 = isbegin_quest 11556
    var q11557 = isbegin_quest 11557
    if (q11556 == 1)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Вставайте в очередь! "
        wait
        choose menu "Спросить, почему здесь очередь " "Уйти "
        case 1
        break
        endchoose
        dialog "[Плюшевый мишка в очереди] "
        dialog "Я что, похож на того, кто выдаст такие важные сведения первому встречному? "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Если вам так интересно, вставайте за мной! Сами все узнаете, когда придет ваша очередь! "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Информация - это не то, что можно получить задаром. "
        wait
        dialog "[???] "
        dialog "А-а-а-а-а!!! "
        dialog "Пустите меня! "
        dialog "Лохматые кретины! "
        wait
        dialog "Вы слышите доносящийся издалека крик. Выясните, что произошло. "
        completequest 11556
        setquest 11557
        close
        return
    endif
    if (q11557 == 1)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Мне кажется, или я слышу какой-то странный звук? "
        wait
        dialog "Вы слышите доносящийся издалека крик. Выясните, что произошло. "
        close
        return
    else
        dialog "[Плюшевый мишка в очереди] "
        dialog "Вставайте в очередь! "
        close
        return
    endif
return

npc "ein_dun01" "Плюшевый мишка#IBT1 " 4_NASARIAN 189 85 7 0 0
OnInit:
    AddQuestIDCondition 11557
    SetQuestConditionBegin 11557 1 0
    SetQuestConditionQuest 11557 1
    SetQuestConditionEnd
return

OnClick:
    var q11557 = isbegin_quest 11557
    var q11558 = isbegin_quest 11558
    if (q11557 == 0)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Хе-хе, скоро моя очередь. "
        close
        return
    endif
    if (q11557 == 1)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Спрашиваете, что случилось? "
        dialog "Как только зашел тот, кто стоял передо мной, я услышал крики. "
        wait
        choose menu "Спросить о стоявшем перед ним "
        case 1
        break
        endchoose
        dialog "[Плюшевый мишка в очереди] "
        dialog "Что? Не помню! Я его не рассматривал! "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Хм... Я стоял в очереди несколько дней, но... отчего-то не хочу здесь больше находиться. "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Если вам интересно, можете занять мое место и сходить проверить, что там. Что скажете? "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Я вовсе не струсил! Просто мне надоело тут стоять. Мне это больше не интересно, понятно?! "
        completequest 11557
        setquest 11558
        close
        return
    endif
    if (q11558 == 1)
        dialog "[Плюшевый мишка в очереди] "
        dialog "Хм... Я стоял в очереди несколько дней, но... отчего-то не хочу здесь больше находиться. "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Если вам интересно, можете занять мое место и сходить проверить, что там. Что скажете? "
        wait
        dialog "[Плюшевый мишка в очереди] "
        dialog "Я вовсе не струсил! Просто мне надоело тут стоять. Мне это больше не интересно, понятно?! "
        close
        return
    else
        dialog "[Плюшевый мишка в очереди] "
        dialog "Что?! Тот, кто стоял передо мной, уже ушел! "
        dialog "Нечего меня путать! "
        close
        return
    endif
return

npc "ein_dun01" "Мишка#3 " 4_NASARIAN 188 81 7 0 0
OnClick:
    dialog "[Плюшевый мишка в очереди] "
    dialog "Эй, там! Не лезьте без очереди! "
    close
    return

npc "ein_dun01" "Мишка#4 " 4_NASARIAN 189 82 7 0 0
npc "ein_dun01" "Мишка#5 " 4_NASARIAN 190 80 7 0 0
npc "ein_dun01" "Мишка#6 " 4_NASARIAN 191 80 5 0 0
npc "ein_dun01" "Мишка#7 " 4_NASARIAN 193 80 5 0 0

npc "ein_dun01" "Житель#8 " 4_M_EINMAN2 195 80 5 0 0
OnClick:
    dialog "[Житель] "
    dialog "Однажды все здесь выстроились в очередь... И я со всеми. Чем я хуже? "
    wait
    dialog "[Житель] "
    dialog "Спрашиваете, что впереди? "
    dialog "Неизвестно. Я тоже хочу это узнать, потому и стою здесь. "
    close
    return

npc "ein_dun01" "Мишка#9 " 4_NASARIAN 196 80 1 0 0
OnClick:
    dialog "[Плюшевый мишка в очереди] "
    dialog "Все стоят здесь несколько дней в ожидании своей очереди. "
    close
    return

npc "ein_dun01" "Вход#IBTin " 4_ENERGY_BLUE 189 87 3 0 0
OnClick:
    var q11558 = isbegin_quest 11558
    if (q11558 > 0)
        moveto "ein_d02_i" 168 150
        return
    else
        dialog "Войти пока нельзя. "
        close
        return
    endif
return

```

## Задание 2
Потребовалось ~ 1,5 ч \
В функции `GetNonSlotItemSock2` было не слишком понятно какое значение брать для `<reflvl>`, поэтому по умолчанию поставила значение 1.  
```
DisableItemMove

// Начало взаимодействия игрока с NPC
OnClick:
    dialog "[Усилитель реликвий] "
    dialog "Доброго времени суток! Ну что, хотите обменять руны или улучшить какой-нибудь предмет? "
    choose menu "Обменять руны на предмет " "Улучшить предмет " "Уйти "
    case 1
        dialog "Выберите предмет, который хотите получить: "
        local item_choice = choose menu 
            "Хлыст бесконечности" 
            "Скрипка бесконечности"
            "Сюрикен бесконечности"
            "Револьвер бесконечности"
            "Кинжал бесконечности"
            "Двуручный посох бесконечности"
            "Дубинка бесконечности"
            "Двуручный меч бесконечности"
            "Топор бесконечности"
            "Лук бесконечности"
            "Древняя броня из Разлома"
            "Ботинки из Разлома"
            "Накидка из Разлома"
            "Древнее украшение из Разлома"

        local required_runes = 50
        local item_name
        
        switch item_choice
            case 1: item_name = "Whip_Of_Infinite"
            case 2: item_name = "Viollin_Of_Infinite"
            case 3: item_name = "Huuma_Of_Infinite"
            case 4: item_name = "Gun_Of_Infinite"
            case 5: item_name = "Dagger_Of_Infinite"
            case 6: item_name = "D_Staff_Of_Infinite"
            case 7: item_name = "Mace_Of_Infinite"
            case 8: item_name = "D_Sword_Of_Infinite"
            case 9: item_name = "Axe_Of_Infinite"
            case 10: item_name = "Bow_Of_Infinite"
            case 11: item_name = "Armor_Of_Goddess"
            case 12: item_name = "Shoes_Of_Cracks"
            case 13: item_name = "ManteauOfCracks"
            case 14: item_name = "Accessories_Of_Goddess"
        endchoose
        
        // Проверка наличия рун у игрока
        if (v["Shattered_Rune"] >= required_runes)
            // Проверка, поместится ли предмет в инвентарь
            if (GetInventoryRemainCount(item_name, 1) == 1)
                dropitem "Shattered_Rune" required_runes
                getitem item_name 1
                dialog ("Вы получили " .. item_name .. "!")
            else
                dialog "Ваш инвентарь полон, не хватает места для нового предмета! "
            endif
        else
            dialog "У вас недостаточно рун для обмена! "
        endif
        
        break
    
    case 2
        local required_runes = 30
        // Улучшение предмета
        dialog "Улучшение происходит за " .. required_runes .. " рун. Выберите предмет для улучшения: "
        
        // Здесь можно добавить предметы для улучшения, например:
        local upgrade_item_choice = choose menu 
            "Топор бесконечности (Чары силы I)" 
            "Двуручный меч бесконечности (Чары силы II)"
            "Двуручный посох бесконечности (Чары боевого духа I)" 
            "Сюрикен бесконечности (Чары боевого духа II)" 
            "Лук бесконечности (Чары боевого духа III)" 
            "Револьвер бесконечности (Чары боевого духа IV)" 
            "Дубинка бесконечности (Чары боевого духа V)" 
            "Кинжал бесконечности (Чары боевого духа VI)" 
            "Скрипка бесконечности (Чары боевого духа VII)" 
            "Хлыст бесконечности (Чары боевого духа VIII)"
        
        local item_name
        local enhance_name
        local db_name_enc

        switch upgrade_item_choice
            case 1: 
                item_name = "Axe_Of_Infinite"
                enhance_name = "Чары силы I"
                db_name_enc = "Strength1"
            case 2: 
                item_name = "D_Sword_Of_Infinite"
                enhance_name = "Чары силы II"
                db_name_enc = "Strength2"
            case 3: 
                item_name = "D_Staff_Of_Infinite"
                enhance_name = "Чары боевого духа I"
                db_name_enc = "Fighting_Spirit1"
            case 4: 
                item_name = "Huuma_Of_Infinite"
                enhance_name = "Чары боевого духа II"
                db_name_enc = "Fighting_Spirit2"
            case 5: 
                item_name = "Bow_Of_Infinite"
                enhance_name = "Чары боевого духа III"
                db_name_enc = "Fighting_Spirit3"
            case 6: 
                item_name = "Gun_Of_Infinite"
                enhance_name = "Чары боевого духа IV"
                db_name_enc = "Fighting_Spirit4"
            case 7: 
                item_name = "Mace_Of_Infinite"
                enhance_name = "Чары боевого духа V"
                db_name_enc = "Fighting_Spirit5"
            case 8: 
                item_name = "Dagger_Of_Infinite"
                enhance_name = "Чары боевого духа VI"
                db_name_enc = "Fighting_Spirit6"
            case 9: 
                item_name = "Viollin_Of_Infinite"
                enhance_name = "Чары боевого духа VII"
                db_name_enc = "Fighting_Spirit7"
            case 10: 
                item_name = "Whip_Of_Infinite"
                enhance_name = "Чары боевого духа VIII"
                db_name_enc = "Fighting_Spirit8"
        endchoose
        
        // Проверка наличия предмета и рун для улучшения
        if (v[item_name] > 0)
            if (v["Shattered_Rune"] >= 30) // Проверка наличия 30 рун
                if (rand(0, 100) <= 30) // 30% шанс улучшения
                    dropitem item_name 1
                    dropitem "Shattered_Rune" 30
                    getNonSlotItemSock2(1, item_name, 1, 0, 0, 0)
                    dialog "Предмет успешно улучшен! "
                else
                    dialog "Улучшение не удалось. Попробуйте снова."
                endif
            else
                dialog "У вас недостаточно рун для улучшения! Необходимо " .. required_runes .. " рун. "
            endif
        else
            dialog "У вас нет этого предмета для улучшения! "
        endif
        
        break
    
    case 3
        dialog "Всего доброго, буду ждать Вас снова! "
        close
        break
    
    endchoose

EnableItemMove

```
