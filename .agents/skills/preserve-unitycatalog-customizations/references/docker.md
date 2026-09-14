# dd86c798 — Docker cache and operational notes

`Dockerfile` adds `ENV COURSIER_CACHE=$HOME/.cache/coursier` before `WORKDIR $HOME`. The recorded rationale is that resolved dependency paths must exist in the non-root runtime image. Preserve cache/classpath alignment across build and runtime stages. Validate startup as the intended runtime user, not just image build success.

This commit also creates `AGENT.md` with branch context and Docker/Harbor publishing examples. Its later local relocation to `.agents/skills/AGENT.md` is separate from this historical commit.

