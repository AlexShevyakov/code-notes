<template src="./NoteCard.html">
</template>

<script>
import Vuex from 'vuex';
import editor from '../../editor/Editor';
import UpdateNoteModal from '../../modals/update-note-modal/UpdateNoteModal';
import BTooltip from '../../../../../node_modules/buefy/src/components/tooltip/Tooltip.vue';

export default {
  name: 'cn-note-card',
  components: {
    BTooltip,
    'cn-update-note-modal': UpdateNoteModal,
    editor,
  },
  props: {
    note: Object,
  },
  mounted() {},
  data() {
    return {
      updateNoteModalActive: false,
    };
  },
  computed: {
    ...Vuex.mapGetters(['notes', 'gistsSelected']),
    displayNoteName() {
      return this.note.name.split('-')[0];
    },
  },
  methods: {
    updateNote() {
      this.$store.dispatch('updateNote', this.note);
    },
    deleteNote() {
      this.$dialog.confirm({
        title: this.gistsSelected ? 'Delete gist' : 'Delete note',
        message: `Are you sure you want to delete this ${this.gistsSelected ? 'gist': 'note'} ?`,
        confirmText: 'Delete',
        type: 'is-danger',
        hasIcon: true,
        onConfirm: () => {
          this.$store.dispatch('deleteNote', this.note);
        },
      });
    },
    onCopyClipboardSuccess() {
      this.$toast.open({
        message: 'Copied',
        position: 'is-bottom',
      });
    },
    open(link) {
      this.$electron.shell.openExternal(link);
    },
  },
};
</script>

<style src="./NoteCard.scss" lang="scss" scoped>

</style>
