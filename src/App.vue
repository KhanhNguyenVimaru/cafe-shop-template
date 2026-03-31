<script setup lang="ts">
import {
  Disclosure,
  DisclosureButton,
  DisclosurePanel,
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
  Tab,
  TabGroup,
  TabList,
} from '@headlessui/vue'
import { computed, onMounted, onUnmounted, ref, Transition } from 'vue'

type Slide = {
  title: string
  subtitle: string
  image: string
}

type Product = {
  name: string
  description: string
  price: string
  image: string
}

type StoryBlock = {
  title: string
  description: string
  cta: string
  image: string
}

const navItems = ['Trang chủ', 'Thực đơn', 'Ưu đãi', 'Liên hệ']
const slides: Slide[] = [
  {
    title: 'Cà Phê Rang Mộc',
    subtitle: 'Hương thơm dịu, vị đậm cân bằng cho ngày mới trọn vẹn.',
    image:
      'https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?auto=format&fit=crop&w=1400&q=80',
  },
  {
    title: 'Không Gian Ấm Cúng',
    subtitle: 'Thiết kế tone cafe nhẹ nhàng cho làm việc và gặp gỡ bạn bè.',
    image:
      'https://images.unsplash.com/photo-1501339847302-ac426a4a7cbb?auto=format&fit=crop&w=1400&q=80',
  },
  {
    title: 'Bánh Ngọt Tươi',
    subtitle: 'Kết hợp hoàn hảo cùng cappuccino và cold brew đặc trưng.',
    image:
      'https://images.unsplash.com/photo-1554118811-1e0d58224f24?auto=format&fit=crop&w=1400&q=80',
  },
]

const products: Product[] = [
  {
    name: 'Espresso Mộc',
    description: 'Đậm vị, hậu ngọt nhẹ, phù hợp cho buổi sáng tỉnh táo.',
    price: '45.000đ',
    image:
      'https://images.unsplash.com/photo-1510707577719-ae7c14805e3a?auto=format&fit=crop&w=900&q=80',
  },
  {
    name: 'Latte Caramel',
    description: 'Cân bằng giữa espresso, sữa tươi và caramel thơm dịu.',
    price: '55.000đ',
    image:
      'https://images.unsplash.com/photo-1494314671902-399b18174975?auto=format&fit=crop&w=900&q=80',
  },
  {
    name: 'Cold Brew Cam',
    description: 'Cold brew ủ lạnh 16 giờ kết hợp cam tươi, vị thanh mát.',
    price: '59.000đ',
    image:
      'https://images.unsplash.com/photo-1461023058943-07fcbe16d735?auto=format&fit=crop&w=900&q=80',
  },
  {
    name: 'Mocha Hạt Dẻ',
    description: 'Chocolate đen, espresso và kem sữa hạt dẻ, thơm béo vừa phải.',
    price: '62.000đ',
    image:
      'https://images.unsplash.com/photo-1578314675249-a6910f80cc4e?auto=format&fit=crop&w=900&q=80',
  },
]

const storyBlocks: StoryBlock[] = [
  {
    title: 'CÀ PHÊ ESPRESSO CỦA CHÚNG TÔI, LATTE CỦA BẠN',
    description:
      'Cà phê espresso đậm vị và bốc hơi sữa, ngon trọn vẹn khi thưởng thức riêng hoặc thêm chút đường nâu caramel.',
    cta: 'Hãy thưởng thức ngay',
    image:
      'https://images.unsplash.com/photo-1517701604599-bb29b565090c?auto=format&fit=crop&w=1200&q=80',
  },
  {
    title: 'CƠ HỘI',
    description: 'Không chỉ là nhân viên, mà còn là cộng sự trên hành trình tạo trải nghiệm cà phê mỗi ngày.',
    cta: 'Tham gia cùng chúng tôi',
    image:
      'https://images.unsplash.com/photo-1556740738-b6a63e27c4df?auto=format&fit=crop&w=1200&q=80',
  },
]

const fallbackSlide: Slide = {
  title: 'Cafe Mộc',
  subtitle: 'Không gian ấm áp và cà phê rang mộc mỗi ngày.',
  image:
    'https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?auto=format&fit=crop&w=1400&q=80',
}

const activeIndex = ref(0)
let timer: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  timer = setInterval(() => {
    activeIndex.value = (activeIndex.value + 1) % slides.length
  }, 20000)
})

onUnmounted(() => {
  if (timer) {
    clearInterval(timer)
  }
})

const setSlide = (index: number) => {
  activeIndex.value = index
}

const currentSlide = computed<Slide>(() => slides[activeIndex.value] ?? fallbackSlide)
</script>

<template>
  <div class="min-h-screen w-full bg-[var(--coffee-cream)] text-[var(--coffee-ink)]">
    <div class="h-1 w-full bg-[var(--coffee-accent)]/80"></div>
    <Disclosure
      as="header"
      class="sticky top-0 z-30 w-full border-b border-[var(--coffee-brown)]/10 bg-white/95 text-[var(--coffee-ink)] shadow-sm backdrop-blur"
      v-slot="{ open }"
    >
      <div class="mx-auto flex w-full max-w-8xl items-center justify-between px-4 py-4 md:px-8">
        <div class="flex items-center gap-3">
          <div class="h-10 w-10 rounded-full bg-[var(--coffee-accent)]/20 ring-1 ring-[var(--coffee-accent)]/40" />
          <div>
            <p class="text-lg font-bold tracking-wide text-[var(--coffee-brown)]">Cafe Mộc</p>
            <p class="text-xs text-slate-500">Coffee & Bakery</p>
          </div>
        </div>

        <nav class="hidden items-center gap-6 md:flex">
          <a
            v-for="item in navItems"
            :key="item"
            href="#"
            class="text-sm font-medium text-slate-700 transition hover:text-[var(--coffee-brown)]"
          >
            {{ item }}
          </a>
          <Menu as="div" class="relative">
            <MenuButton
              class="rounded-full border border-[var(--coffee-brown)]/30 bg-[var(--coffee-cream)] px-4 py-2 text-sm font-semibold text-[var(--coffee-brown)] transition hover:border-[var(--coffee-brown)] hover:bg-[var(--coffee-brown)] hover:text-[var(--coffee-cream)]"
            >
              Đặt bàn
            </MenuButton>
            <MenuItems
              class="absolute right-0 mt-2 w-44 rounded-xl bg-white p-2 text-[var(--coffee-ink)] shadow-xl focus:outline-none"
            >
              <MenuItem v-slot="{ active }">
                <button
                  :class="[
                    'w-full rounded-lg px-3 py-2 text-left text-sm',
                    active ? 'bg-[var(--coffee-cream)]' : '',
                  ]"
                >
                  Đặt bàn 2 người
                </button>
              </MenuItem>
              <MenuItem v-slot="{ active }">
                <button
                  :class="[
                    'w-full rounded-lg px-3 py-2 text-left text-sm',
                    active ? 'bg-[var(--coffee-cream)]' : '',
                  ]"
                >
                  Đặt bàn nhóm
                </button>
              </MenuItem>
              <MenuItem v-slot="{ active }">
                <button
                  :class="[
                    'w-full rounded-lg px-3 py-2 text-left text-sm',
                    active ? 'bg-[var(--coffee-cream)]' : '',
                  ]"
                >
                  Gọi mang đi
                </button>
              </MenuItem>
            </MenuItems>
          </Menu>
        </nav>

        <DisclosureButton class="rounded-md border border-[var(--coffee-brown)]/30 bg-[var(--coffee-cream)] px-3 py-2 text-sm text-[var(--coffee-brown)] md:hidden">
          {{ open ? 'Đóng' : 'Menu' }}
        </DisclosureButton>
      </div>

      <DisclosurePanel class="space-y-2 px-4 pb-4 md:hidden">
        <a
          v-for="item in navItems"
          :key="item"
          href="#"
          class="block rounded-lg bg-[var(--coffee-cream)] px-3 py-2 text-sm text-[var(--coffee-brown)]"
        >
          {{ item }}
        </a>
      </DisclosurePanel>
    </Disclosure>

    <main class="w-full">
      <TabGroup :selected-index="activeIndex" @change="setSlide">
        <section class="relative w-full">
          <div class="relative h-[360px] w-full overflow-hidden md:h-[520px]">
            <Transition
              mode="out-in"
              enter-active-class="transition-transform transition-opacity duration-700 ease-out"
              enter-from-class="translate-x-full opacity-80"
              enter-to-class="translate-x-0 opacity-100"
              leave-active-class="absolute inset-0 transition-transform transition-opacity duration-700 ease-in"
              leave-from-class="translate-x-0 opacity-100"
              leave-to-class="-translate-x-full opacity-80"
            >
              <div :key="currentSlide.title" class="absolute inset-0">
                <img
                  :src="currentSlide.image"
                  :alt="currentSlide.title"
                  class="h-full w-full object-cover"
                />
                <div class="absolute inset-0 bg-gradient-to-r from-[var(--coffee-brown)]/80 to-transparent" />
                <div class="absolute inset-0 flex items-center">
                  <div class="mx-auto w-full max-w-8xl px-4 md:px-8">
                    <div class="max-w-xl space-y-4 text-[var(--coffee-cream)]">
                      <p class="text-sm uppercase tracking-[0.3em] text-[var(--coffee-accent)]">Specialty Coffee</p>
                      <h1 class="text-3xl font-bold leading-tight md:text-5xl">{{ currentSlide.title }}</h1>
                      <p class="text-sm md:text-base">{{ currentSlide.subtitle }}</p>
                      <button
                        class="rounded-full bg-[var(--coffee-accent)] px-6 py-3 text-sm font-semibold text-white transition hover:brightness-110"
                      >
                        Xem thực đơn
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </Transition>
          </div>

          <TabList
            class="absolute bottom-5 left-1/2 flex -translate-x-1/2 gap-2 rounded-full bg-black/25 px-3 py-2"
          >
            <Tab v-for="slide in slides" :key="slide.title" v-slot="{ selected }" as="template">
              <button
                :class="[
                  'h-3 w-3 rounded-full transition',
                  selected ? 'bg-[var(--coffee-accent)]' : 'bg-white/60',
                ]"
              />
            </Tab>
          </TabList>
        </section>
      </TabGroup>

      <section class="mx-auto w-full max-w-8xl px-4 py-12 md:px-8">
        <div class="mb-6 flex items-end justify-between gap-4">
          <div>
            <p class="text-sm uppercase tracking-[0.2em] text-[var(--coffee-accent)]">Signature Menu</p>
            <h2 class="text-2xl font-bold text-[var(--coffee-brown)] md:text-3xl">Sản phẩm nổi bật</h2>
          </div>
          <button
            class="rounded-full border border-[var(--coffee-brown)] px-4 py-2 text-sm font-semibold text-[var(--coffee-brown)] transition hover:bg-[var(--coffee-brown)] hover:text-[var(--coffee-cream)]"
          >
            Xem tất cả
          </button>
        </div>

        <div class="grid gap-4 md:grid-cols-4">
          <article
            v-for="product in products"
            :key="product.name"
            class="overflow-hidden rounded-2xl bg-white shadow-sm transition hover:-translate-y-1 hover:shadow-lg"
          >
            <img :src="product.image" :alt="product.name" class="h-48 w-full object-cover" />
            <div class="space-y-2 p-5">
              <h3 class="text-lg font-bold text-[var(--coffee-brown)]">{{ product.name }}</h3>
              <p class="text-sm text-slate-700">{{ product.description }}</p>
              <p class="pt-1 text-sm font-semibold text-[var(--coffee-accent)]">{{ product.price }}</p>
            </div>
          </article>
        </div>
      </section>

      <section class="w-full bg-[#ececec] py-16 md:py-24">
        <div
          v-for="(block, idx) in storyBlocks"
          :key="block.title"
          class="mx-auto mb-14 grid w-full max-w-8xl items-center gap-10 px-4 md:mb-18 md:grid-cols-2 md:px-8"
        >
          <div
            class="overflow-hidden border-8 border-white shadow-md"
            :class="idx % 2 === 1 ? 'md:order-1' : 'md:order-2'"
          >
            <img :src="block.image" :alt="block.title" class="h-[300px] w-full object-cover md:h-[360px]" />
          </div>
          <div :class="idx % 2 === 1 ? 'md:order-2' : 'md:order-1'">
            <h3 class="max-w-lg text-2xl font-extrabold uppercase leading-tight text-black md:text-[2rem]">{{ block.title }}</h3>
            <p class="mt-6 max-w-xl text-lg/8 italic text-slate-900 md:text-xl/9">{{ block.description }}</p>
            <a
              href="#"
              class="mt-8 inline-block border-b border-black pb-1 text-2xl/none font-semibold text-black transition hover:text-[var(--coffee-brown)] hover:border-[var(--coffee-brown)] md:text-3xl/none"
            >
              {{ block.cta }} »
            </a>
          </div>
        </div>
      </section>
    </main>

    <footer class="w-full bg-[var(--coffee-brown)] text-[var(--coffee-cream)]">
      <div
        class="mx-auto flex w-full max-w-8xl flex-col gap-2 px-4 py-8 text-sm md:flex-row md:items-center md:justify-between md:px-8"
      >
        <p>© 2026 Cafe Mộc. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>
