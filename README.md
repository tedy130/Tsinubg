export default {
  async fetch(request, env, ctx) {
    const url = new URL(request.url);
    const key = url.pathname.slice(1);

    if (!key) {
      return new Response("እባክዎ የሚፈልጉትን ፋይል ስም ይጥቀሱ", { status: 400 });
    }

    const object = await env.Buks.get(key);

    if (!object) {
      return new Response("ፋይሉ አልተገኘም", { status: 404 });
    }

    const headers = new Headers();
    object.writeHttpMetadata(headers);
    headers.set("etag", object.httpEtag);
    headers.set("Access-Control-Allow-Origin", "*");

    return new Response(object.body, {
      headers,
    });
  },
};
