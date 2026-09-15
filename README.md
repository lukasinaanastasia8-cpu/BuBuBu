import React, { useMemo, useState } from "react";
import {
  View,
  Text,
  StyleSheet,
  TouchableOpacity,
  ScrollView,
  Modal,
  TextInput,
  Alert,
  Image,
  SafeAreaView,
} from "react-native";

const STAS_PHOTO =
  "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAA4KCw0LCQ4NDA0QDw4RFiQXFhQUFiwgIRokNC43NjMuMjI6QVNGOj1OPjIySGJJTlZYXV5dOEVmbWVabFNbXVn/2wBDAQ8QEBYTFioXFypZOzI7WVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVlZWVn/wAARCADcANwDASIAAhEBAxEB/8QAGwAAAgMBAQEAAAAAAAAAAAAAAgQBAwUABgf/xAA4EAACAgEDAwMDAQcBCAMAAAABAgADEQQhMQUSQRMiUTJhcYEGFBVCUpGhIxYkJTNDcoLBk7HR/8QAGQEAAwEBAQAAAAAAAAAAAAAAAAECAwQF/8QAHxEBAQEBAQADAAMBAAAAAAAAAAERAhIDITEEQVET/9oADAMBAAIRAxEAPwD5+N8zpAkwUkecmRmSo5z8SIgkbnEidJgHSZAEf6d023XWbZWocv8A/kLcMtRTZfYEqXuael0Gl0/TSGC+vqcbt4WNU6GnTVCupAPk+TLlpCjYTLrv/GvPxX+1Nuo1epPusKL/AErtATSAsS2TkeZp16fg4h+mAZGtZwzhpUUbLOOlUjiPGuSK4tPwzTpscCAdMpbcTVNWZH7vHo8Mo6YBg1ZKMPiNVazX1Ick2oOc8x1dOAOBL6K1XIIGDDU346W0Ot0+obFjdlvw0fZxjC+YpqemU3jI9rfaJJqdT0yzsuX1KfB8iNneLGyqYGWliqW+y/MDS206qr1UcMvkfH5hXW4TY4T+r5/EeoFZalW1Y7n+PA/MUNjWMe0h347vA/E5Ua0EfTXnJzyfzCLKlZ7SqVqPc52AgTlRa2z9dh8TP6r1ejp6nvIt1P8AKg4X8zK6t+0YVWo6f52a48n8TzDMzMWZizHkk5zLkLTeu6hqOoXepqLCfhRwIrOELEoimZOYMkSgIHmROE6IOzJhV1vbYErUs52AE9X0r9nxpQt+sAa47qh4X8wtkm05LfxldM6O9/bZeCtZ4U8meoopWpAiL2qPAlwTfJEsVJz9d66uPjwIqENUHxLAsLtka2xwEhhvCAnQCvthhZOJMDR2j4ndskQsQAMSRtCxOxEEAznRLUK2KCDOkjaOVNmsfV6K3RP6+kYgeV+fyJdoeo16xibzi8bdp4/SaL+5CDuDPP8AUdEa7PUqyp5BE0ljDvhq6/X0aOnv1Ldo/lrHLTx/Ves39RPbk10jhFP/ANyvqK33Wmyx2sb7zPGZrI56kTpwhgRk4CFCWtm8S1att4aMZUkThJjCYzodDfr7xVQhJ8nwPzHej9Dv6m4Y5rpHLEc/ie/6d0yjQUCulAo8nyZN6xU51m9I6JR0un1G99pG7n/1GWJtfuP6S/VW99vpL9K8/cyvEw6uun4+c+whYaiQBCElumTzBJhCATididOgNdOnE4kA5PEAmSJIkwCQJxWSJMQD2yCIeIJgAGL3oHQgjMZIlL7COI6ec1Onw5BGxmLrdIa8ug28z1epTuHHmZ19WQQRNea5uuXmlVm4GZcKscxlq/TJAGIHmaazxwhyBJiVjJVSSABkz0/Q/wBmmu7b9YuF5VD/AO5q9D/ZyvSBbdQBZd/hZ6QKtYGBvF11/hSBo06UIoVQABsB4lXUNUNPST/MdlHyZZZYEUs7YE8/Ze2s1JuP0A4Qfb5mbSTTVROMnk8xgbiKqcDeErWMNl2+ZLoi4sJKmAB5PMsAiWnGYYgiFiMOnTjsJOIiQRIxCnYgEAwhvO7YQWBiEmQJ0QdIYSZxgAHiVWD2y4iAw2hCrPsG+DEbk3ORNK0bxSxSeZpGHUYWtrAOREyMTY1leAdpkPgGXGVQDJzBBEnuHkxk+nZA2X+8ouuFew9zfEC2/B7K928n4g11Y3bcmQZDqlrLQA31W7D7CKVjtRQJPUrjZrynioAD8zuAIq14hleIffhRiAN12kgZktlgOZasrQS5QIHEgQu2cohiIwEQgIWJ0YDidiTJBgTgIWJEIQN2JBG0KREEYkQxBIgAwGEs4gmIFLVwYrYPMfdMnMSsBBMqVl1GbqF7wduYpo9DVqFuNqnuVgF32xNJhmH01VLWK/AOdpesLNZGs6fRp6ncbYHOY5pejVW6etwvd3AHM7qFdWo1ldNhb0id56Hp2oq0eiroSvKL9JPxDR4qyusKQAMmXOVqXL7t8Qi60jC+5zyZRYmKbLLDgBSf8QS8uthsvewnJdi3+Y6x9uZm0HJGI8WyMSa6OTVbe0S1TFajGlGwzEtaph5lPcBODQw9MK2/MMMIt3zlt8+IYNNhs7TmODKUsyQZFr4t+2Ixq0tOBlXeJxsx5EAYB2nd4G8TbUAcGVWawKvOY5CvTS78yPUGdzMV+oBeDA/iHccHmGF6bjXKokpcHmH+8MwJzLKNQ6vvx8xWHOmwxzOgV2B0Bk5kKcRmJ6hD35jmZRqh7QY4npnWDBMTq1tejtd7VZl49sdsG+Zh9SHtP/dNIx3Lqdd1D1LUs0nfWwJyWAM01/aDTBFDU2EgDO885OjxX/a+tfTVqWkFrPcx+fEQ6jc1tFiLvlSMfpLGse5tjzOapVrfb3FSP8RMI8nptnjWYtpvpJPMYXdoq6OTVR7R3HiG2qUDxFdRYqL25mdbdzgwwW41bNcqjMpHU0zgGYV1tjHziUh2ErEeq9IOoKTzL01IbgzzCOfJjdWoIIzDDnT0tNuTgwrm/wBUfiZen1G43mgx7mU/aS0lXqciL6mwqIyinEU1amAZ9+of08DYmZj3W/JmhcPEVdVP1HEqIpdGutOMZ+8e0+ksIyxMGq2lDzv9poU3KV2DH9I6UiEpZV5zDXuWWfvCgbqwH4ltbV2D2sDIVInT6jtfGJoA5GRM/wBHDbcR2r6BJq4tEDUDNRx4hrJcZQiI7GRZuTMbqown6ibL7MR8GK26RdW5Ru4DI4lyspx6uR53M7M37ugoKiyeox5xkTP/AHKv+m3+8vY35/g/J3+WPcjtQbDP2khCxBbiEiYAJ5loUAZY4ElwvHel6Vtqf0uR/mSpwcy/XY/ier7fp7sj+0W8RN+fwlqXJc5MqrQs2/Em7eww1ZKl9x3lRNWDTqw3g2aJcZBxK21qp9h8wa+oq16VkKoc473Ow+5jLS9qCsnfYSsWfeNu76nvrOmAKjJx8fMznHa+OIy1oae/3Ceg0r+oFM8pSxDies6XXmlWPMnppx9tbTr7NxKNcmVzjxGquJGqUtUZDXHltRlcmZ1hHd7zk/AnoNdphZSCmzZmSNGUsznJHzLjPqE7bbaAMIFyM5xvKK+oXs4DWMqk8gTUv05tK5PHxI0/SKS5LFiD4lMrL/SnT63VFWsQ91anGCI5XrFtYCypkbwyRynRVogQAdvxGq9OgxhBt9pNac81VprzwWLj5ImlUR2ytKQPEvVMfiZ1rIsUZhSBtCI2kqZGpTGsAGfdHEqWpckDuk21j94qsPgkSxSC+SMgSkyMY6q460u2Qmdh4Ah3UdthxuDuI7rNOCCwH3naYj0QGGcbRtds/GzlaxlpRa5c4O58KPH5kEs7YQ5Plvj8SrUXppamCnLeSY3nMbXr29QuBxkhTE2zkxi9jZezk5LASrtyYnRJ9M+0YbOIk622OSFLGbx0yvuRA9IJwuB+JUpXnWJ+6Wsh9pzKm0V5sDLWZvZUEwlBPCsf0lek+CnTtM+lLWWHuYr2jfMX1GlWy9nbk+BNhaHYbjAknSFvENPwwqNI73AY9onpdE3psEHHEGvTrWoAAzCpX/XEjq6055xsVcS4gFcShIwOJDXGbdp/cy+DELdAc5GR+JvOgYShkxHKnGD+53A7N/eHXp9QP5lmuyj4ghN5Wl5JJQ4+uzP2jNSYG2ZaK88iWKm4ipyOQS3t2kqsMiSpXiT4h4gHYxBTeM1n7bzJr6gx1BVdkzNaz3DExr9L6F20qFGyjrbWAcREJ2M6/DSNFYe8Axp0yxMKqD1+vo0OnLWHtHgDkzzB6g+vtJs9q59qDgSu3Wpq7P8Aek7wf5vIkrpK0sW6i8FBuVPM0xwc/q7R2GwOGOew4jtag4iGjHaG23f3TRo5Eiuuw2lIIxiWDTrjcQqWEviOQsdOg4USDSPAjfbBIgeKBWB4kOMDaWkyiywAbxixW20LT1ZbulYJsfEfoXC/aKlFqDEtBlYlhQlOMRLSIDYhMp7PvFLe5RmAWkCCAMxNdVnnmXJbk/cysI0FhACVo+0sVswA1E6dnadJCIDCETBJ2gFTcxfqCjCN5IjDjeL6gCy4KfAECL6Rc2/iPSK6hWuBCxHVPMf7P645wijH3hv0nVaWh7LQvaBjY5nqLNXUGIGoI/8ACIa+6uygr67Oc/T24zL1xcT7YVfs1YrJ/kGI/SfcJmO3/EkP6TSqODJrqrQpPtEbU7TPrfaXeocRHDLNiCX2lPeTOL7RnomfESD+tey52XmW2uAhMw11nZdcQcZxHiLW29tVAO+8pr6mAcBuZganXvgnOcxKvU2M3BEfkvT3dWrV1BzLW1wUbnaeS02ssGOY097vXhSQT5hh+npK9alg3OIbWVup3HE8SNPrFckWvgza6ct7jD57fMWKlTflWzjaHRd3eY7fSrVYImS6Gts+IFrYrsz5lyuZl0ORjeNq5+Yhp1XhhxiJh5Yr/MVPVxO87MDunZ2kgLHeSiKT3EbzmxgyxB7RAkEAwO3PBhsQM7w6KabKwz3hCfGJUhddY+fjW3n+ez+8v0mod9QoYuc/JiodfgS2mwCxSANjNccvP6010wsZrG2KnIlqNiGbFWvHkxbMh06bWzB5lot35mabMeZatoI5gWtFXz5hd28SrcfMvRwTBWg17FdO3bzPPus9BeyupVjsZlWUAn2GVEWkTSSndzJRANppVaQ+mATzGaNAvcpI7jHqSen0rWcbDyZo0abhQN/mP1aUKvxLK6sNkCTrSRWmlA54jFSqq4EuFYxuZ3pDwYjC+MfaIX1qwOI86HcRHUZQHMCpb6DGUbuGxmf6qWntHMPSWMt5Q/MClaDAgQq2+YZGUH5k+n7s+JNWNYU5RtCOMSTA/EzT1itbHUn2qcCOaqwV0O5OygmeRtZ76zZW4yDuuJfM1l115aWp6ytjFey3tPlTgxX+I1/06r/5Ilpta9bYYqR9xG2cWnvB2PwMTTGHVvTJBhqxG4lfEkGUGzReLEVieBvLEYMCRMRGZD7SRH+n2lg6tzzIsazpdZIR8GWPvK1XJEkzatsPvLHs7F53gsu1OPmWX6ckbRgrZd5zBpIZwOIlqlupYkjKjjEVGsbJXDKZUhPRnUVVcnJ+JK9UCHKpMKmrVXnFFZZjxLf4b1FrAH2wd/tDFyPQp1is/UuJw6mWz24A+Zm19K1DoQCCwHzL9F0+zuVSQxMMaSDfqBLElmgfxhK/aLHyftHn6J/1Lm7ARjGZ1OhoXZEGfxBU50oOsWgHAJH3EldTq9cuEp7QeWaaSaFTyBiNJUtY2EnU2POv016m7wx7s7/EaqqIuDfaad65i6oFaTrPDCcDMMSoNxLOcyVDEF2kZlbt+kQ0h1q3t6faPLYUTylNhrbO+OCJ6TrJzpQfAcTzt6ANtwZtww7K24W9gPpzkRmpz2DeUagYrVxyDgyanzWMS2afM458ScbH8SpmPzGa2W6V/TvU+DsYrW5PMuHiKwa1iczhBHAkzNqcrYNWATwdo9TYGXcDMyayRxHqiQMiI06ipXUqwGDMiylUft7Bj5mwTnmL2opY7RyjCyH08dhK4+Iymosx9RMUbYmWITHrWdY0tNqLFZTnzLEVs5XmI1OwOI4jtjmC/R1VJGXJJ+My+kKo+8zxYw8xuncSaPRsENxJI2gpxJJiQptWUERqziUESUgGc5hKcHM7G0h9lMA5jKrHwYRO8XtOYEU6vkdLJ+XWYN4yimeh6yM9K/DKZhlQaSTNOWfRC4Z0tn2wYGn/AOUJdYP9CwfiAAAMDiaM3//Z";

const C = {
  bg: "#06111B",
  panel: "#0B1B29",
  panel2: "#102638",
  border: "#17496A",
  blue: "#2A9FD6",
  gold: "#FFB82E",
  green: "#16D7B1",
  red: "#FF5277",
  white: "#F5F8FA",
  muted: "#8DA2B3",
};

const categories = ["Топливо", "Еда", "Ремонт", "Стоянка", "Дорога", "Другое"];

const money = (n) =>
  `${Math.round(Number(n) || 0).toLocaleString("ru-RU")} ₽`;

function Avatar({ size = 58 }) {
  return (
    <View
      style={[
        styles.avatarWrap,
        { width: size, height: size, borderRadius: size / 2 },
      ]}
    >
      <Image
        source={{ uri: STAS_PHOTO }}
        style={{ width: size, height: size, borderRadius: size / 2 }}
      />
    </View>
  );
}

function Header({ title, onBack }) {
  return (
    <View style={styles.header}>
      {onBack ? (
        <TouchableOpacity onPress={onBack} style={styles.back}>
          <Text style={styles.backText}>‹</Text>
        </TouchableOpacity>
      ) : (
        <Avatar size={48} />
      )}

      <View style={{ flex: 1 }}>
        <Text style={styles.headerTitle}>{title}</Text>
        <Text style={styles.headerSub}>Водительский учёт</Text>
      </View>

      <Text style={styles.truckSmall}>🚛</Text>
    </View>
  );
}

function StatCard({ icon, title, value, color = C.white }) {
  return (
    <View style={styles.statCard}>
      <Text style={styles.statIcon}>{icon}</Text>
      <Text style={styles.statTitle}>{title}</Text>
      <Text style={[styles.statValue, { color }]}>{value}</Text>
    </View>
  );
}

function Button({ title, onPress, secondary = false }) {
  return (
    <TouchableOpacity
      onPress={onPress}
      style={[styles.mainButton, secondary && styles.secondaryButton]}
    >
      <Text style={[styles.buttonText, secondary && { color: C.gold }]}>
        {title}
      </Text>
    </TouchableOpacity>
  );
}

function Home({ trips, expenses, go }) {
  const revenue = trips.reduce((s, x) => s + Number(x.amount), 0);
  const share = revenue * 0.25;
  const spent = expenses.reduce((s, x) => s + Number(x.amount), 0);
  const left = share - spent;

  return (
    <ScrollView style={styles.screen} contentContainerStyle={styles.content}>
      <View style={styles.hero}>
        <View style={styles.heroTop}>
          <Avatar size={68} />
          <View style={{ flex: 1, marginLeft: 14 }}>
            <Text style={styles.heroTitle}>Рейсы Стаса</Text>
            <Text style={styles.heroSub}>Дальнобой • деньги • свобода</Text>
          </View>
          <View style={styles.percent}>
            <Text style={styles.percentText}>25%</Text>
          </View>
        </View>

        <View style={styles.road}>
          <Text style={styles.bigTruck}>🚛</Text>
          <Text style={styles.roadText}>ДОРОГА ДАЁТ СВОБОДУ</Text>
        </View>
      </View>

      <Text style={styles.section}>Сентябрь 2026</Text>

      <View style={styles.grid}>
        <StatCard icon="🚛" title="Всего за рейсы" value={money(revenue)} />
        <StatCard icon="👤" title="Наши 25%" value={money(share)} color={C.green} />
        <StatCard icon="💳" title="Траты" value={money(spent)} color={C.red} />
        <StatCard
          icon="💰"
          title="Осталось"
          value={money(left)}
          color={left >= 0 ? C.green : C.red}
        />
      </View>

      <View style={styles.sectionRow}>
        <Text style={styles.section}>Последние рейсы</Text>
        <TouchableOpacity onPress={() => go("trips")}>
          <Text style={styles.link}>Все →</Text>
        </TouchableOpacity>
      </View>

      {trips.slice(0, 4).map((t) => (
        <View style={styles.tripCard} key={t.id}>
          <Text style={styles.tripIcon}>🚛</Text>
          <View style={{ flex: 1 }}>
            <Text style={styles.small}>{t.date}</Text>
            <Text style={styles.route}>
              {t.from} → {t.to}
            </Text>
            <Text style={styles.ourShare}>Наши 25%: {money(t.amount * 0.25)}</Text>
          </View>
          <Text style={styles.tripMoney}>{money(t.amount)}</Text>
        </View>
      ))}

      <Button title="＋  Добавить рейс" onPress={() => go("trips", true)} />

      <View style={styles.quote}>
        <Text style={styles.quoteText}>«Дорога — это работа.</Text>
        <Text style={styles.quoteText}>А цифры должны быть под контролем.»</Text>
      </View>
    </ScrollView>
  );
}

function Trips({ trips, setTrips, openAdd }) {
  const [edit, setEdit] = useState(null);

  const remove = (id) => {
    Alert.alert("Удалить рейс?", "Рейс будет удалён.", [
      { text: "Отмена", style: "cancel" },
      {
        text: "Удалить",
        style: "destructive",
        onPress: () => setTrips((a) => a.filter((x) => x.id !== id)),
      },
    ]);
  };

  return (
    <ScrollView style={styles.screen} contentContainerStyle={styles.content}>
      <Header title="Рейсы" />

      <Button title="＋  Добавить рейс" onPress={openAdd} />

      {trips.length === 0 && (
        <View style={styles.empty}>
          <Text style={styles.emptyIcon}>🚛</Text>
          <Text style={styles.emptyTitle}>Пока нет рейсов</Text>
          <Text style={styles.emptyText}>Добавьте первый рейс Стаса</Text>
        </View>
      )}

      {trips.map((t) => (
        <View style={styles.tripCardLarge} key={t.id}>
          <View style={styles.tripTop}>
            <Text style={styles.small}>{t.date}</Text>
            <Text style={styles.tripMoney}>{money(t.amount)}</Text>
          </View>

          <Text style={styles.routeBig}>
            {t.from}  →  {t.to}
          </Text>

          <View style={styles.shareLine}>
            <Text style={{ color: C.muted }}>Наши 25%</Text>
            <Text style={{ color: C.green, fontWeight: "800" }}>
              {money(t.amount * 0.25)}
            </Text>
          </View>

          <View style={styles.actionRow}>
            <TouchableOpacity
              style={styles.action}
              onPress={() => setEdit(t)}
            >
              <Text style={styles.actionText}>✏️ Изменить</Text>
            </TouchableOpacity>

            <TouchableOpacity
              style={[styles.action, { borderColor: "#5B2030" }]}
              onPress={() => remove(t.id)}
            >
              <Text style={[styles.actionText, { color: C.red }]}>
                🗑 Удалить
              </Text>
            </TouchableOpacity>
          </View>
        </View>
      ))}

      <TripModal
        visible={!!edit}
        trip={edit}
        onClose={() => setEdit(null)}
        onSave={(updated) => {
          setTrips((a) => a.map((x) => (x.id === updated.id ? updated : x)));
          setEdit(null);
        }}
      />
    </ScrollView>
  );
}

function Expenses({ expenses, setExpenses, openAdd }) {
  const total = expenses.reduce((s, x) => s + Number(x.amount), 0);

  const remove = (id) => {
    Alert.alert("Удалить трату?", "", [
      { text: "Отмена", style: "cancel" },
      {
        text: "Удалить",
        style: "destructive",
        onPress: () => setExpenses((a) => a.filter((x) => x.id !== id)),
      },
    ]);
  };

  const sums = {};
  expenses.forEach((e) => {
    sums[e.category] = (sums[e.category] || 0) + Number(e.amount);
  });

  return (
    <ScrollView style={styles.screen} contentContainerStyle={styles.content}>
      <Header title="Траты" />

      <View style={styles.expenseTotal}>
        <Text style={styles.expenseLabel}>Всего потрачено</Text>
        <Text style={styles.expenseMoney}>{money(total)}</Text>
      </View>

      <Text style={styles.section}>Куда уходят деньги</Text>

      {categories.map((cat, i) => {
        const value = sums[cat] || 0;
        const percent = total ? Math.round((value / total) * 100) : 0;

        return (
          <View style={styles.categoryRow} key={cat}>
            <Text style={styles.categoryIcon}>
              {["⛽", "🍔", "🔧", "🅿️", "🛣️", "📦"][i]}
            </Text>

            <View style={{ flex: 1 }}>
              <View style={styles.catLine}>
                <Text style={styles.categoryName}>{cat}</Text>
                <Text style={styles.categoryAmount}>{money(value)}</Text>
              </View>
              <View style={styles.progressBg}>
                <View style={[styles.progress, { width: `${percent}%` }]} />
              </View>
            </View>

            <Text style={styles.percentCat}>{percent}%</Text>
          </View>
        );
      })}

      <Button title="＋  Добавить трату" onPress={openAdd} />

      <Text style={styles.section}>История расходов</Text>

      {expenses.map((e) => (
        <View style={styles.expenseRow} key={e.id}>
          <Text style={styles.expenseEmoji}>💳</Text>
          <View style={{ flex: 1 }}>
            <Text style={styles.small}>{e.date}</Text>
            <Text style={styles.expenseCat}>{e.category}</Text>
            {e.comment ? <Text style={styles.small}>{e.comment}</Text> : null}
          </View>
          <View style={{ alignItems: "flex-end" }}>
            <Text style={styles.redMoney}>{money(e.amount)}</Text>
            <TouchableOpacity onPress={() => remove(e.id)}>
              <Text style={styles.deleteText}>удалить</Text>
            </TouchableOpacity>
          </View>
        </View>
      ))}

      <ExpenseModal
        visible={false}
        onClose={() => {}}
        onSave={() => {}}
      />
    </ScrollView>
  );
}

function Statistics({ trips, expenses }) {
  const revenue = trips.reduce((s, x) => s + Number(x.amount), 0);
  const our = revenue * 0.25;
  const spent = expenses.reduce((s, x) => s + Number(x.amount), 0);

  const sums = {};
  expenses.forEach((e) => {
    sums[e.category] = (sums[e.category] || 0) + Number(e.amount);
  });

  const max = Math.max(...Object.values(sums), 1);

  return (
    <ScrollView style={styles.screen} contentContainerStyle={styles.content}>
      <Header title="Статистика" />

      <View style={styles.monthBox}>
        <Text style={styles.monthArrow}>‹</Text>
        <Text style={styles.monthText}>Сентябрь 2026</Text>
        <Text style={styles.monthArrow}>›</Text>
      </View>

      <Text style={styles.section}>Расходы по категориям</Text>

      <View style={styles.chartBox}>
        {categories.map((cat, i) => {
          const val = sums[cat] || 0;
          const h = Math.max(8, (val / max) * 130);

          return (
            <View style={styles.barCol} key={cat}>
              <Text style={styles.barValue}>{val ? Math.round(val / 1000) + "к" : ""}</Text>
              <View style={styles.barArea}>
                <View style={[styles.bar, { height: h }]} />
              </View>
              <Text style={styles.barName}>{cat.slice(0, 6)}</Text>
            </View>
          );
        })}
      </View>

      <Text style={styles.section}>Итоги месяца</Text>

      <View style={styles.summary}>
        <View style={styles.summaryItem}>
          <Text style={styles.summaryIcon}>🚛</Text>
          <Text style={styles.summaryValue}>{money(revenue)}</Text>
          <Text style={styles.summaryLabel}>Все рейсы</Text>
        </View>

        <View style={styles.summaryItem}>
          <Text style={styles.summaryIcon}>👤</Text>
          <Text style={[styles.summaryValue, { color: C.green }]}>
            {money(our)}
          </Text>
          <Text style={styles.summaryLabel}>Наши 25%</Text>
        </View>

        <View style={styles.summaryItem}>
          <Text style={styles.summaryIcon}>💳</Text>
          <Text style={[styles.summaryValue, { color: C.red }]}>
            {money(spent)}
          </Text>
          <Text style={styles.summaryLabel}>Траты</Text>
        </View>

        <View style={styles.summaryItem}>
          <Text style={styles.summaryIcon}>💰</Text>
          <Text style={styles.summaryValue}>{money(our - spent)}</Text>
          <Text style={styles.summaryLabel}>Осталось</Text>
        </View>
      </View>

      <View style={styles.bigLogo}>
        <Avatar size={100} />
        <Text style={styles.bigLogoTitle}>Рейсы Стаса</Text>
        <Text style={styles.bigLogoSub}>ДАЛЬНОБОЙ • РЕЙСЫ • ДЕНЬГИ</Text>
        <Text style={styles.logoTruck}>🚛</Text>
      </View>
    </ScrollView>
  );
}

function TripModal({ visible, trip, onClose, onSave }) {
  const [date, setDate] = useState(trip?.date || "");
  const [from, setFrom] = useState(trip?.from || "");
  const [to, setTo] = useState(trip?.to || "");
  const [amount, setAmount] = useState(trip ? String(trip.amount) : "");

  React.useEffect(() => {
    if (visible) {
      setDate(trip?.date || new Date().toLocaleDateString("ru-RU"));
      setFrom(trip?.from || "");
      setTo(trip?.to || "");
      setAmount(trip ? String(trip.amount) : "");
    }
  }, [visible, trip]);

  const save = () => {
    if (!date || !from || !to || !amount) {
      Alert.alert("Заполни все поля");
      return;
    }

    onSave({
      id: trip?.id || Date.now().toString(),
      date,
      from,
      to,
      amount: Number(amount.replace(/\s/g, "").replace(",", ".")),
    });
  };

  return (
    <Modal visible={visible} animationType="slide" transparent>
      <View style={styles.modalBg}>
        <View style={styles.modal}>
          <Text style={styles.modalTitle}>
            {trip ? "Изменить рейс" : "Новый рейс"}
          </Text>

          <TextInput
            style={styles.input}
            placeholder="Дата"
            placeholderTextColor={C.muted}
            value={date}
            onChangeText={setDate}
          />

          <TextInput
            style={styles.input}
            placeholder="Откуда"
            placeholderTextColor={C.muted}
            value={from}
            onChangeText={setFrom}
          />

          <TextInput
            style={styles.input}
            placeholder="Куда"
            placeholderTextColor={C.muted}
            value={to}
            onChangeText={setTo}
          />

          <TextInput
            style={styles.input}
            placeholder="Сумма рейса, ₽"
            placeholderTextColor={C.muted}
            keyboardType="numeric"
            value={amount}
            onChangeText={setAmount}
          />

          {amount ? (
            <View style={styles.preview}>
              <Text style={{ color: C.muted }}>Наши 25%</Text>
              <Text style={styles.previewMoney}>
                {money(Number(amount.replace(",", ".")) * 0.25)}
              </Text>
            </View>
          ) : null}

          <Button title="Сохранить рейс" onPress={save} />
          <Button title="Отмена" secondary onPress={onClose} />
        </View>
      </View>
    </Modal>
  );
}

function ExpenseModal({ visible, onClose, onSave }) {
  const [date, setDate] = useState("");
  const [category, setCategory] = useState("Топливо");
  const [amount, setAmount] = useState("");
  const [comment, setComment] = useState("");

  const save = () => {
    if (!date || !amount) {
      Alert.alert("Заполни дату и сумму");
      return;
    }

    onSave({
      id: Date.now().toString(),
      date,
      category,
      amount: Number(amount.replace(",", ".")),
      comment,
    });

    setDate("");
    setAmount("");
    setComment("");
  };

  return (
    <Modal visible={visible} animationType="slide" transparent>
      <View style={styles.modalBg}>
        <View style={styles.modal}>
          <Text style={styles.modalTitle}>Новая трата</Text>

          <TextInput
            style={styles.input}
            placeholder="Дата"
            placeholderTextColor={C.muted}
            value={date}
            onChangeText={setDate}
          />

          <Text style={styles.inputLabel}>Категория</Text>

          <View style={styles.categoryPicker}>
            {categories.map((x) => (
              <TouchableOpacity
                key={x}
                onPress={() => setCategory(x)}
                style={[
                  styles.categoryButton,
                  category === x && styles.categorySelected,
                ]}
              >
                <Text
                  style={{
                    color: category === x ? C.gold : C.white,
                    fontSize: 12,
                  }}
                >
                  {x}
                </Text>
              </TouchableOpacity>
            ))}
          </View>

          <TextInput
            style={styles.input}
            placeholder="Сумма, ₽"
            placeholderTextColor={C.muted}
            keyboardType="numeric"
            value={amount}
            onChangeText={setAmount}
          />

          <TextInput
            style={styles.input}
            placeholder="Комментарий (необязательно)"
            placeholderTextColor={C.muted}
            value={comment}
            onChangeText={setComment}
          />

          <Button title="Сохранить" onPress={save} />
          <Button title="Отмена" secondary onPress={onClose} />
        </View>
      </View>
    </Modal>
  );
}

export default function App() {
  const [tab, setTab] = useState("home");

  const [trips, setTrips] = useState([
    {
      id: "1",
      date: "10.09.2026",
      from: "Москва",
      to: "Казань",
      amount: 250000,
    },
    {
      id: "2",
      date: "08.09.2026",
      from: "СПб",
      to: "Москва",
      amount: 180000,
    },
    {
      id: "3",
      date: "05.09.2026",
      from: "Ростов",
      to: "Краснодар",
      amount: 220000,
    },
  ]);

  const [expenses, setExpenses] = useState([
    {
      id: "1",
      date: "10.09.2026",
      category: "Топливо",
      amount: 35000,
      comment: "АЗС",
    },
    {
      id: "2",
      date: "08.09.2026",
      category: "Еда",
      amount: 15000,
      comment: "Обед",
    },
    {
      id: "3",
      date: "06.09.2026",
      category: "Стоянка",
      amount: 8000,
      comment: "Платная стоянка",
    },
    {
      id: "4",
      date: "04.09.2026",
      category: "Ремонт",
      amount: 12000,
      comment: "Замена масла",
    },
  ]);

  const [tripModal, setTripModal] = useState(false);
  const [expenseModal, setExpenseModal] = useState(false);

  const saveTrip = (trip) => {
    setTrips((old) => {
      const exists = old.some((x) => x.id === trip.id);
      return exists ? old.map((x) => (x.id === trip.id ? trip : x)) : [trip, ...old];
    });
    setTripModal(false);
  };

  const saveExpense = (expense) => {
    setExpenses((old) => [expense, ...old]);
    setExpenseModal(false);
  };

  const screen = useMemo(() => {
    if (tab === "trips") {
      return (
        <Trips
          trips={trips}
          setTrips={setTrips}
          openAdd={() => setTripModal(true)}
        />
      );
    }

    if (tab === "expenses") {
      return (
        <Expenses
          expenses={expenses}
          setExpenses={setExpenses}
          openAdd={() => setExpenseModal(true)}
        />
      );
    }

    if (tab === "stats") {
      return <Statistics trips={trips} expenses={expenses} />;
    }

    return (
      <Home
        trips={trips}
        expenses={expenses}
        go={(where, add) => {
          setTab(where);
          if (add) setTimeout(() => setTripModal(true), 150);
        }}
      />
    );
  }, [tab, trips, expenses]);

  return (
    <SafeAreaView style={styles.app}>
      {screen}

      <View style={styles.nav}>
        <Nav
          icon="⌂"
          title="Главная"
          active={tab === "home"}
          onPress={() => setTab("home")}
        />
        <Nav
          icon="🚛"
          title="Рейсы"
          active={tab === "trips"}
          onPress={() => setTab("trips")}
        />
        <Nav
          icon="▣"
          title="Траты"
          active={tab === "expenses"}
          onPress={() => setTab("expenses")}
        />
        <Nav
          icon="▥"
          title="Статистика"
          active={tab === "stats"}
          onPress={() => setTab("stats")}
        />
      </View>

      <TripModal
        visible={tripModal}
        onClose={() => setTripModal(false)}
        onSave={saveTrip}
      />

      <ExpenseModal
        visible={expenseModal}
        onClose={() => setExpenseModal(false)}
        onSave={saveExpense}
      />
    </SafeAreaView>
  );
}

function Nav({ icon, title, active, onPress }) {
  return (
    <TouchableOpacity onPress={onPress} style={styles.navItem}>
      <Text style={[styles.navIcon, active && { color: C.gold }]}>{icon}</Text>
      <Text style={[styles.navText, active && { color: C.gold }]}>
        {title}
      </Text>
    </TouchableOpacity>
  );
}

const styles = StyleSheet.create({
  app: {
    flex: 1,
    backgroundColor: C.bg,
  },

  screen: {
    flex: 1,
    backgroundColor: C.bg,
  },

  content: {
    padding: 16,
    paddingBottom: 100,
  },

  header: {
    flexDirection: "row",
    alignItems: "center",
    marginBottom: 18,
    paddingTop: 6,
  },

  headerTitle: {
    color: C.white,
    fontSize: 23,
    fontWeight: "900",
  },

  headerSub: {
    color: C.muted,
    fontSize: 12,
    marginTop: 2,
  },

  avatarWrap: {
    borderWidth: 2,
    borderColor: C.gold,
    overflow: "hidden",
    backgroundColor: C.panel2,
  },

  back: {
    width: 45,
    height: 45,
    borderRadius: 23,
    backgroundColor: C.panel,
    alignItems: "center",
    justifyContent: "center",
    marginRight: 10,
    borderWidth: 1,
    borderColor: C.border,
  },

  backText: {
    color: C.white,
    fontSize: 35,
    lineHeight: 38,
  },

  truckSmall: {
    fontSize: 30,
  },

  hero: {
    backgroundColor: C.panel,
    borderRadius: 22,
    borderWidth: 1,
    borderColor: C.border,
    padding: 16,
    overflow: "hidden",
    marginBottom: 18,
  },

  heroTop: {
    flexDirection: "row",
    alignItems: "center",
  },

  heroTitle: {
    color: C.white,
    fontSize: 25,
    fontWeight: "900",
  },

  heroSub: {
    color: C.muted,
    marginTop: 4,
  },

  percent: {
    width: 50,
    height: 50,
    borderRadius: 25,
    backgroundColor: "#172B3B",
    borderWidth: 2,
    borderColor: C.gold,
    alignItems: "center",
    justifyContent: "center",
  },

  percentText: {
    color: C.gold,
    fontWeight: "900",
  },

  road: {
    marginTop: 18,
    height: 90,
    backgroundColor: "#07131E",
    borderRadius: 15,
    justifyContent: "center",
    alignItems: "center",
    borderWidth: 1,
    borderColor: "#163A52",
  },

  bigTruck: {
    fontSize: 46,
  },

  roadText: {
    color: C.gold,
    fontWeight: "900",
    fontSize: 11,
    letterSpacing: 2,
    marginTop: 2,
  },

  section: {
    color: C.white,
    fontSize: 18,
    fontWeight: "800",
    marginTop: 10,
    marginBottom: 10,
  },

  sectionRow: {
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
  },

  link: {
    color: C.gold,
    fontWeight: "800",
  },

  grid: {
    flexDirection: "row",
    flexWrap: "wrap",
    justifyContent: "space-between",
  },

  statCard: {
    width: "48.5%",
    backgroundColor: C.panel,
    borderRadius: 16,
    borderWidth: 1,
    borderColor: C.border,
    padding: 13,
    marginBottom: 10,
    minHeight: 115,
  },

  statIcon: {
    fontSize: 25,
    marginBottom: 5,
  },

  statTitle: {
    color: C.muted,
    fontSize: 12,
  },

  statValue: {
    fontSize: 19,
    fontWeight: "900",
    marginTop: 7,
  },

  tripCard: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: C.panel,
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 14,
    padding: 12,
    marginBottom: 8,
  },

  tripIcon: {
    fontSize: 28,
    marginRight: 11,
  },

  small: {
    color: C.muted,
    fontSize: 11,
  },

  route: {
    color: C.white,
    fontSize: 14,
    fontWeight: "700",
    marginTop: 2,
  },

  routeBig: {
    color: C.white,
    fontSize: 19,
    fontWeight: "800",
    marginTop: 8,
  },

  ourShare: {
    color: C.green,
    fontSize: 11,
    marginTop: 4,
    fontWeight: "700",
  },

  tripMoney: {
    color: C.white,
    fontWeight: "900",
    fontSize: 14,
  },

  mainButton: {
    backgroundColor: C.gold,
    minHeight: 50,
    borderRadius: 15,
    alignItems: "center",
    justifyContent: "center",
    marginVertical: 10,
  },

  secondaryButton: {
    backgroundColor: "transparent",
    borderWidth: 1,
    borderColor: C.border,
  },

  buttonText: {
    color: "#101010",
    fontSize: 16,
    fontWeight: "900",
  },

  quote: {
    marginTop: 12,
    padding: 20,
    backgroundColor: "#0A1A27",
    borderRadius: 18,
    borderLeftWidth: 3,
    borderLeftColor: C.gold,
  },

  quoteText: {
    color: C.gold,
    fontSize: 16,
    fontStyle: "italic",
    fontWeight: "700",
    lineHeight: 24,
  },

  tripCardLarge: {
    backgroundColor: C.panel,
    borderRadius: 18,
    borderWidth: 1,
    borderColor: C.border,
    padding: 15,
    marginBottom: 10,
  },

  tripTop: {
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
  },

  shareLine: {
    marginTop: 12,
    paddingTop: 10,
    borderTopWidth: 1,
    borderTopColor: "#173247",
    flexDirection: "row",
    justifyContent: "space-between",
  },

  actionRow: {
    flexDirection: "row",
    gap: 8,
    marginTop: 13,
  },

  action: {
    flex: 1,
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 10,
    padding: 9,
    alignItems: "center",
  },

  actionText: {
    color: C.white,
    fontSize: 12,
    fontWeight: "700",
  },

  empty: {
    alignItems: "center",
    padding: 50,
  },

  emptyIcon: {
    fontSize: 60,
  },

  emptyTitle: {
    color: C.white,
    fontSize: 20,
    fontWeight: "900",
    marginTop: 10,
  },

  emptyText: {
    color: C.muted,
    marginTop: 5,
  },

  expenseTotal: {
    backgroundColor: "#28121C",
    borderColor: "#6B2539",
    borderWidth: 1,
    borderRadius: 18,
    padding: 20,
    marginBottom: 15,
  },

  expenseLabel: {
    color: C.muted,
    fontSize: 13,
  },

  expenseMoney: {
    color: C.red,
    fontSize: 30,
    fontWeight: "900",
    marginTop: 5,
  },

  categoryRow: {
    flexDirection: "row",
    alignItems: "center",
    marginBottom: 14,
  },

  categoryIcon: {
    fontSize: 23,
    width: 40,
  },

  catLine: {
    flexDirection: "row",
    justifyContent: "space-between",
  },

  categoryName: {
    color: C.white,
    fontWeight: "700",
  },

  categoryAmount: {
    color: C.white,
    fontWeight: "700",
    fontSize: 12,
  },

  progressBg: {
    height: 6,
    backgroundColor: "#182B39",
    borderRadius: 4,
    marginTop: 6,
    overflow: "hidden",
  },

  progress: {
    height: 6,
    backgroundColor: C.gold,
    borderRadius: 4,
  },

  percentCat: {
    color: C.muted,
    fontSize: 11,
    width: 35,
    textAlign: "right",
  },

  expenseRow: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: C.panel,
    borderRadius: 14,
    borderWidth: 1,
    borderColor: C.border,
    padding: 12,
    marginBottom: 8,
  },

  expenseEmoji: {
    fontSize: 25,
    marginRight: 10,
  },

  expenseCat: {
    color: C.white,
    fontWeight: "700",
    marginTop: 2,
  },

  redMoney: {
    color: C.red,
    fontWeight: "900",
  },

  deleteText: {
    color: C.muted,
    fontSize: 10,
    marginTop: 5,
  },

  monthBox: {
    height: 48,
    borderWidth: 1,
    borderColor: C.border,
    backgroundColor: C.panel,
    borderRadius: 13,
    flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",
    paddingHorizontal: 15,
  },

  monthText: {
    color: C.white,
    fontWeight: "700",
  },

  monthArrow: {
    color: C.gold,
    fontSize: 28,
  },

  chartBox: {
    height: 220,
    backgroundColor: C.panel,
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 18,
    padding: 12,
    flexDirection: "row",
    alignItems: "flex-end",
    justifyContent: "space-around",
  },

  barCol: {
    flex: 1,
    alignItems: "center",
    justifyContent: "flex-end",
    height: "100%",
  },

  barValue: {
    color: C.muted,
    fontSize: 9,
    height: 20,
  },

  barArea: {
    height: 145,
    justifyContent: "flex-end",
  },

  bar: {
    width: 22,
    backgroundColor: C.gold,
    borderRadius: 5,
    minHeight: 8,
  },

  barName: {
    color: C.muted,
    fontSize: 9,
    marginTop: 6,
  },

  summary: {
    flexDirection: "row",
    flexWrap: "wrap",
    justifyContent: "space-between",
  },

  summaryItem: {
    width: "48%",
    backgroundColor: C.panel,
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 15,
    padding: 12,
    marginBottom: 9,
  },

  summaryIcon: {
    fontSize: 20,
  },

  summaryValue: {
    color: C.white,
    fontSize: 16,
    fontWeight: "900",
    marginTop: 5,
  },

  summaryLabel: {
    color: C.muted,
    fontSize: 10,
    marginTop: 3,
  },

  bigLogo: {
    marginTop: 15,
    backgroundColor: C.panel,
    borderRadius: 22,
    borderWidth: 1,
    borderColor: C.gold,
    alignItems: "center",
    padding: 25,
  },

  bigLogoTitle: {
    color: C.white,
    fontSize: 26,
    fontWeight: "900",
    marginTop: 10,
  },

  bigLogoSub: {
    color: C.gold,
    fontSize: 11,
    fontWeight: "800",
    letterSpacing: 1,
    marginTop: 5,
  },

  logoTruck: {
    fontSize: 42,
    marginTop: 10,
  },

  nav: {
    position: "absolute",
    bottom: 0,
    left: 0,
    right: 0,
    height: 72,
    backgroundColor: "#07131E",
    borderTopWidth: 1,
    borderTopColor: "#17364C",
    flexDirection: "row",
    justifyContent: "space-around",
    paddingTop: 7,
  },

  navItem: {
    alignItems: "center",
    justifyContent: "center",
    width: "25%",
  },

  navIcon: {
    color: C.muted,
    fontSize: 23,
  },

  navText: {
    color: C.muted,
    fontSize: 10,
    marginTop: 3,
  },

  modalBg: {
    flex: 1,
    backgroundColor: "rgba(0,0,0,0.78)",
    justifyContent: "flex-end",
  },

  modal: {
    backgroundColor: "#091925",
    borderTopLeftRadius: 25,
    borderTopRightRadius: 25,
    borderWidth: 1,
    borderColor: C.border,
    padding: 20,
    paddingBottom: 30,
  },

  modalTitle: {
    color: C.white,
    fontSize: 24,
    fontWeight: "900",
    marginBottom: 15,
  },

  input: {
    backgroundColor: C.panel,
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 12,
    color: C.white,
    paddingHorizontal: 14,
    height: 50,
    marginBottom: 10,
  },

  inputLabel: {
    color: C.muted,
    marginBottom: 8,
    fontSize: 12,
  },

  preview: {
    backgroundColor: "#082B2A",
    borderColor: C.green,
    borderWidth: 1,
    borderRadius: 13,
    padding: 13,
    marginBottom: 5,
    flexDirection: "row",
    justifyContent: "space-between",
    alignItems: "center",
  },

  previewMoney: {
    color: C.green,
    fontWeight: "900",
    fontSize: 18,
  },

  categoryPicker: {
    flexDirection: "row",
    flexWrap: "wrap",
    gap: 6,
    marginBottom: 10,
  },

  categoryButton: {
    borderWidth: 1,
    borderColor: C.border,
    borderRadius: 10,
    paddingVertical: 7,
    paddingHorizontal: 9,
    backgroundColor: C.panel,
  },

  categorySelected: {
    borderColor: C.gold,
    backgroundColor: "#2A2110",
  },
});
