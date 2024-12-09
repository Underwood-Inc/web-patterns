<script setup lang="ts">
import { computed } from 'vue';
import { useData } from 'vitepress';

const { site, page, theme } = useData();
const currentYear = new Date().getFullYear();
const repoBase = 'https://github.com/Underwood-Inc/web-patterns';

// Check if current page should have a sidebar
const hasSidebar = computed(() => {
  // Don't show sidebar on home page
  if (page.value.frontmatter.home) return false
  
  // Check if current path matches any sidebar configuration
  const path = page.value.relativePath
  return Object.keys(theme.value.sidebar).some(base => 
    path.startsWith(base.slice(1)) // Remove leading slash for comparison
  )
})

const footerLinks = [
  { text: 'Documentation', link: '/guide/getting-started' },
  { text: 'Examples', link: '/examples/' },
  { text: 'Report Issue', link: `${repoBase}/issues/new/choose` },
  { text: 'Start Discussion', link: `${repoBase}/discussions/new/choose` },
  { text: 'Contributing', link: `${repoBase}/blob/main/CONTRIBUTING.md` },
];

const socialLinks = [
  { icon: 'github', link: repoBase },
  { icon: 'discord', link: 'https://discord.gg/your-server' },
];
</script>

<template>
  <footer class="VPFooter" :class="{ 'has-sidebar': hasSidebar }">
    <div class="container">
      <div class="footer-content">
        <div class="footer-links">
          <div
            v-for="(link, index) in footerLinks"
            :key="index"
            class="footer-link-group"
          >
            <a :href="link.link" class="footer-link" target="_blank" rel="noopener noreferrer">{{ link.text }}</a>
          </div>
        </div>

        <div class="footer-social">
          <a
            v-for="social in socialLinks"
            :key="social.icon"
            :href="social.link"
            class="social-link"
            :aria-label="social.icon"
            target="_blank"
            rel="noopener noreferrer"
          >
            <i :class="`i-${social.icon}`"></i>
          </a>
        </div>

        <div class="footer-copyright">
          <div class="name">
            Released under the <a 
              href="https://www.licenses.ai/blog/2022/8/18/naming-convention-of-responsible-ai-licenses" 
              target="_blank" 
              rel="noopener noreferrer"
            >OpenRAIL-S v1.0</a> License.
          </div>
          <div>Copyright © {{ currentYear }} {{ site.title }}</div>
        </div>
      </div>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
.VPFooter {
  position: relative;
  z-index: var(--vp-z-index-footer);
  border-top: 1px solid var(--vp-c-divider);
  background-color: var(--vp-c-bg-soft);
  padding: 0 24px;

  .container {
    margin: 0 auto;
    padding: 32px 24px;
    max-width: var(--vp-layout-max-width);
  }

  &.has-sidebar {
    padding-left: calc(var(--vp-sidebar-width) + 32px);
  }

  .footer-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
  }

  .footer-links {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .footer-social {
    display: flex;
    gap: 1rem;
    
    .social-link {
      color: var(--vp-c-text-2);
      font-size: 1.2rem;
      
      &:hover {
        color: var(--vp-c-brand);
      }
    }
  }

  .footer-copyright {
    text-align: center;
    color: var(--vp-c-text-2);
    font-size: 0.9rem;
    
    .name {
      background: linear-gradient(120deg, var(--vp-c-brand-light) 30%, var(--vp-c-brand));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      display: inline-block;

      a {
        -webkit-text-fill-color: initial;
        color: var(--vp-c-brand);
        
        &:hover {
          color: var(--vp-c-brand-dark);
        }
      }
    }
  }
}
</style>
