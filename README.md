# expo-finpanel

A Samsung One UI–inspired edge panel — swipe-in quick actions drawer with a second swappable insights page, built for fintech apps.

<!-- HERO GIF/VIDEO — replace with GitHub-hosted asset link -->
<img width="1280" height="720" alt="finpanel" src="PLACEHOLDER" />

---

## ✨ Features

- 📱 **Edge pill trigger** — tap or swipe left on the right-edge pill to open, fades out the moment the drawer opens
- 🎠 **Two-page swipeable drawer** — Quick Actions and Insights live side by side in one carousel, with an animated pill-dot pagination indicator that tracks the active page
- 🪄 **Staggered entrance animations** — each action item springs and fades in with a per-index delay, driven by a small reusable `AnimatedStaggerItem` primitive
- 👆 **Independent, non-conflicting gestures** — pill tap/swipe-to-open, drawer swipe-to-page, and overlay tap-to-dismiss are all handled as separate gesture recognizers so none of them fight each other
- 🧩 **Swappable second page** — `InsightPanel` ships as a placeholder; swap it for spending stats, subscriptions, a savings streak, or anything else, and wrap your content in `AnimatedStaggerItem` to keep the same entrance motion
- 🧠 **TypeScript-first** — typed `ActionItem` list, discriminated `DrawerPage` union (`'actions' | 'insights'`), fully typed props
- ♿ **Accessible by default** — `accessibilityRole` / `accessibilityLabel` on every action item

---

## ⚙️ Installation

This isn't published as an npm package yet — copy the source directly into your project.

```bash
git clone https://github.com/ManasCodeXart/expo-finpanel
```

Copy `src/components/` and `src/constants/` from `src/`, plus `assets/images/` (`Cards.png`, `quickpay.png`, `savings.png`, `crypto.png`, `bills.png`), into your project, then install the peer dependencies:

```bash
npx expo install react-native-reanimated react-native-worklets react-native-gesture-handler
```

> Reanimated 4.x ships its worklets runtime as the separate `react-native-worklets` package — it's required alongside `react-native-reanimated`, not optional.

> Requires `react-native-reanimated`'s Babel plugin already configured, and `GestureHandlerRootView` wrapping your app root — both are standard for any Expo Router / RN project already using Reanimated or Gesture Handler.

---

## 🚀 Usage

```tsx
import { StyleSheet, View } from 'react-native';
import QuickActionsDrawer from './components/QuickActionsDrawer';

export default function Home() {
  return (
    <View style={styles.container}>
      <QuickActionsDrawer
        onActionPress={(id) => console.log('Pressed:', id)}
        onOpenChange={(isOpen) => console.log('Drawer open:', isOpen)}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#0c0c0c' },
});
```

Override the default actions with your own:

```tsx
<QuickActionsDrawer
  actions={[
    { id: 'transfer', label: 'Transfer', icon: require('./assets/transfer.png') },
    { id: 'invest', label: 'Invest', icon: require('./assets/invest.png') },
  ]}
  onActionPress={(id) => navigateTo(id)}
/>
```

## Preview

<!-- PREVIEW VIDEO — replace with GitHub-hosted asset link -->
PLACEHOLDER

---

## 👆 Gestures

| Gesture | Result |
|---|---|
| Tap or swipe left on the edge pill | Opens the drawer |
| Swipe left on the drawer | Advances from Quick Actions → Insights |
| Swipe right on Insights | Goes back to Quick Actions |
| Swipe right on Quick Actions | Closes the drawer |
| Tap the overlay | Closes the drawer from any page |

---

## 🧱 Component Anatomy

```
<QuickActionsDrawer>
  ├─ Pill              (edge trigger — tap or swipe-left to open)
  ├─ Overlay           (backdrop, tap to close)
  └─ Carousel
       ├─ ActionPanel   (page 1 — quick actions grid)
       └─ InsightPanel  (page 2 — placeholder, swap for your own content)
```

`ActionPanel`, `InsightPanel`, and `AnimatedStaggerItem` are also exported individually if you want to use them outside the drawer — see the API tables below.

---

## 🧩 API

### `<QuickActionsDrawer>`

| Prop | Type | Default | Description |
|---|---|---|---|
| `actions` | `readonly ActionItem[]` | 5 built-in fintech actions (Cards, Quick Pay, Savings, Crypto, Your Bills) | Actions rendered in the grid on page one. |
| `onActionPress` | `(actionId: string) => void` | — | Called when an action is tapped. The drawer closes automatically before this fires. |
| `onOpenChange` | `(isOpen: boolean) => void` | — | Called whenever the drawer opens or closes, whether by gesture or overlay tap. |

### `<ActionPanel>`

| Prop | Type | Default | Description |
|---|---|---|---|
| `actions` | `readonly ActionItem[]` | — | Actions to render in the grid. |
| `visible` | `boolean` | — | Controls the stagger entrance animation. |
| `onActionPress` | `(id: string) => void` | — | Fired on tap. |
| `width` | `number` | — | Panel width — driven by the parent drawer's page width. |

### `<InsightPanel>`

| Prop | Type | Default | Description |
|---|---|---|---|
| `visible` | `boolean` | — | Controls the entrance animation. |
| `width` | `number` | — | Panel width — driven by the parent drawer. |

> Ships as a placeholder — swap it for your own content (spending stats, subscriptions, a savings streak, etc.), and wrap it in `AnimatedStaggerItem` to keep the same entrance motion as the rest of the drawer.

### `<AnimatedStaggerItem>`

| Prop | Type | Default | Description |
|---|---|---|---|
| `index` | `number` | — | Position in the stagger sequence. |
| `visible` | `boolean` | — | Triggers entrance (`true`) or resets (`false`). |
| `baseDelay` | `number` | `80` | ms before the first item starts entering. |
| `staggerMs` | `number` | `60` | ms added per subsequent index. |
| `translateDistance` | `number` | `40` | px the item translates in from. |
| `springConfig` | `WithSpringConfig` | `{ damping: 18, stiffness: 220, mass: 0.6 }` | Spring driving the translate. |
| `fadeConfig` | `WithTimingConfig` | `{ duration: 180 }` | Timing driving the opacity fade. |
| `children` | `ReactNode` | — | Content to animate in. |

### Types

```ts
interface ActionItem {
  readonly id: string;
  readonly label: string;
  readonly icon: ImageSourcePropType;
}

type DrawerPage = 'actions' | 'insights';
```

---

## 📄 License

MIT — see [LICENSE](./LICENSE).

---

## 🧱 Stack

[Expo SDK 57](https://expo.dev/changelog) · [React Native 0.86](https://reactnative.dev/) · [Reanimated 4.5](https://docs.swmansion.com/react-native-reanimated/) · [React Native Worklets 0.10](https://docs.swmansion.com/react-native-reanimated/) · [Gesture Handler 2.32](https://docs.swmansion.com/react-native-gesture-handler/)
