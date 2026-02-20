export default async function handler(req, res) {
    if (req.method !== 'POST') return res.status(405).json({ error: 'Method Not Allowed' });

    const { task, value } = req.body;
    const GITHUB_TOKEN = process.env.GH_TOKEN; 
    const REPO_OWNER = "dinzmods"; // SESUAIIN SAMA USERNAME GITHUB LU!
    const REPO_NAME = "DinzXWare"; // SESUAIIN SAMA NAMA REPO LU!
    const FILE_PATH = "status.json";

    try {
        const getFile = await fetch(`https://api.github.com/repos/${REPO_OWNER}/${REPO_NAME}/contents/${FILE_PATH}`, {
            headers: { Authorization: `token ${GITHUB_TOKEN}` }
        });
        const fileData = await getFile.json();
        const sha = fileData.sha;
        let content = JSON.parse(Buffer.from(fileData.content, 'base64').toString());

        content[task] = value;

        const updateFile = await fetch(`https://api.github.com/repos/${REPO_OWNER}/${REPO_NAME}/contents/${FILE_PATH}`, {
            method: 'PUT',
            headers: { Authorization: `token ${GITHUB_TOKEN}`, 'Content-Type': 'application/json' },
            body: JSON.stringify({
                message: `Update ${task} by Dinz`,
                content: Buffer.from(JSON.stringify(content, null, 2)).toString('base64'),
                sha: sha
            })
        });

        if (updateFile.ok) return res.status(200).json({ success: true });
        else return res.status(500).json({ success: false, error: "Gagal update GitHub" });
    } catch (err) {
        return res.status(500).json({ success: false, error: err.message });
    }
}
