
行った変更点 
# カレンダー予約状況のバグ修正
変更前

```
 @event_rooms = RansackUtil.paged_scope(@q, params)
      .where(
        'started_at <= ? and finished_at >= ?',
        start_date + 7.day,
        start_date - 7.day
      )
    @room_calendars = @event_rooms.includes(event_room: :event_parties)
```
変更後
```
 @event_rooms = RansackUtil.paged_scope(@q, params)
    @room_calendars = EventRoomCalendar.includes(event_room: :event_parties)
    .where(
      'started_at <= ? and finished_at >= ?',
      start_date + 7.day,
      start_date - 7.day
    )
```

@room_calendarsを@event_roomsから取得するのではなくEventRoomCalendarから取得する様に変更しました。


# イベント編集の際のエラー修正
変更前 
```
      informate_was_array = @event.informate_was.split(',').map(&:strip)
      @event.my_building_users = @event.my_building_users - informate_was_array
      if @event.my_building_users.present?
        @event.my_building_users =  @event.my_building_users.reject(&:empty?)
        @event.my_building_users.each do |user_id|
```
変更後
```
    if @event.informate_was.present?
        informate_was_array = @event.informate_was.split(',').map(&:strip)
        @event.my_building_users = @event.my_building_users - informate_was_array
      end      

if @event.my_building_users.present?
        @event.my_building_users =  @event.my_building_users.reject(&:empty?)
        @event.my_building_users.each do |user_id|
```
通知機能を実装するために

@event.informateをeventテーブルに追加しました。

@event.informateを追加する前のイベントだと

@event.informate_wasがnilになってしまい、エラーが発生していたので

if @event.informate_was.present?

で条件分岐を行いました。


上記二つがreleaseにmergeした変更点です

以下はまだ現在releaseにはmergeしていないが

dbに対して新しくカラムを追加した修正点です


# customer_staffテーブルにtel_nohyphenカラムの追加
```
t.string :tel_nohyphen, null: true #tel ハイフンなし
```
customer_staffテーブルにtel_nohyphenカラムを追加しました。

script/insert_nohypen_customer_staff_tel.rb
に

```
i = 0
max = CustomerStaff.where.not(tel: "").count
CustomerStaff.where.not(tel: "").find_each do |c|
    i+=1
    puts "#{i}/#{max}"
    c.update_columns(tel_nohyphen: c.tel.to_s.gsub(/-/, ''))
  end
```
  
以下のコードを追加し、

telカラムのレコードを

tel_nohyphenに-がない状態で追加しました。
